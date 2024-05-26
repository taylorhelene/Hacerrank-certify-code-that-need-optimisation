## Challenge 1:

Passed test cases:

![question 1](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)
![test cases](https://github.com/taylorhelene/Hackerrank-certify-solutions-with-explanations/blob/main/Api%20Intermediate/images/day1/Api%20intermediate.PNG?raw=true)

We are supposed to get the number of draws:

1. The total goals are conttrained to 10;
2. Use api to fetch matches drawn by comparing goals
3. Total of ten matches per page . Only the last page may have varying array length in data
4. Use total to sum up draws.

let axios = require('axios') 
/*
 * Complete the 'getNumDraws' function below.
 *
 * The function is expected to return an INTEGER.
 * The function accepts INTEGER year as parameter.
 */

async function getNumDraws(year) {
    
    let iterator = 0;
    
    let lastpage  = 0;
    
    let total =0;
    
    while(iterator <= 10 ){
        
         let promises = await axios.get(`https://jsonmock.hackerrank.com/api/football_matches?year=${year}&team1goals=${iterator}&team2goals=${iterator}`);
    
            let response  = promises.data
   
                lastpage = response.total_pages;
                
                if(lastpage !=0){
                    
                    let promise2 = await axios.get(`https://jsonmock.hackerrank.com/api/football_matches?year=${year}&team1goals=${iterator}&team2goals=${iterator}&page=${lastpage}`);
                
                    let response2 = promise2.data.data.length
                
                    total  = total + ((lastpage -1)* 10) + response2
                
                    console.log(total)
                }
             
        
        iterator ++;
    }
    
    return total;
   

}

## Challenge 2:

We are supposed to get winner total goals:

1. Fetch winner
2. Fetch winners total pages as team1
3. Use iterator for each page and use reduce to find total goals of winner in each page as team 1
2. Fetch winners total pages as team2
3. Use iterator for each page and use reduce to find total goals of winner in each page as team 2

let axios = require('axios')

async function getWinnerTotalGoals(competition, year) {
    
    let promises = await axios.get(`https://jsonmock.hackerrank.com/api/football_competitions?name=${competition}&year=${year}`)
    
    let response = promises.data
    
    let winner = response.data[0].winner
    
    //console.log(winner)
    
    let promisetwo = await axios.get(`https://jsonmock.hackerrank.com/api/football_matches?competition=${competition}&year=${year}&team1=${winner}`)
    
    let response2 = promisetwo.data
    
    let pages = response2.total_pages
    
    let sum = 0;
    
    let iterator = 1;
    
    while(iterator <= pages){
        
        let promisethree = await axios.get(`https://jsonmock.hackerrank.com/api/football_matches?competition=${competition}&year=${year}&team1=${winner}&page=${iterator}`)
        
        
        let arraydata = promisethree.data.data
        
        //console.log(arraydata)
        
        let summed = arraydata.reduce((total,curent)=>{
            
            return total + Number.parseInt(curent.team1goals);
            
        },0)
        
        sum = sum + summed;
        
    
        iterator++;
    }
    
   
    
     let promiseteam2 = await axios.get(`https://jsonmock.hackerrank.com/api/football_matches?competition=${competition}&year=${year}&team2=${winner}`)
    
    let responseteam2 = promiseteam2.data
    
    let pagesteam2 = responseteam2.total_pages
    
    
    let iterator2 = 1;
    
    while(iterator2 <= pagesteam2){
        
        let promisethree = await axios.get(`https://jsonmock.hackerrank.com/api/football_matches?competition=${competition}&year=${year}&team2=${winner}&page=${iterator2}`)
        
        
        let arraydata = promisethree.data.data
        
        
        let summed = arraydata.reduce((total,curent)=>{
            
            return total + Number.parseInt(curent.team2goals);
            
        },0)
        
        sum = sum + summed;
        
    
        iterator2++;
    }
    
    return sum

}

