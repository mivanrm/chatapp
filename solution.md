## Basic Questions
Imagine you're building a website that allows users to submit photos. One of the requirements is that each photo must be reviewed by a moderator before it can be published. How would you design the logic for this process? What technologies would you use? Do you have any data structure in mind to support this based on your technology of choice to handle those data? 
### Logic Flow:

#### user submits photo: 
1. The user uploads a photo through your website or app
2. The photo is stored on server(local storage/s3)
3. Add database record with information of the photo, and status of waiting for moderation


#### Moderator:
1. open an page that contain list of photo waiting for moderation
2. open a single page that contain the photo and allow moderator to review, and do action of approve/reject, and give the reason
3. update the status to the database


### data structure :
1. users: id, role,name
2. photos: id, user_id, url, status,moderator_id,review_reason,creatd_at, updated_at

### technologies 
1.frontend : vue/react
2. backend: node.js, golang,python
3. database: postgresql
4. data storage: amazon s3/local server storage

### consideration 
need to consider multiple moderator working on 1 photo, can be fixed by implementing lock system, can be by updating the row in the database if being moderated, or can use redis to add record of photo:id so other moderator cant access if the key still not expired


## Database Question

1. SELECT count(*) as Answer FROM Customers where country = "Germany";
Output = 11

2. SELECT country, count(*) as Answer FROM Customers  group by country having count(*) >=5 order by count(*) desc;

3. SELECT c.customerName, count(*) as OrderCount, MIN(o.OrderDate) as FirstOrderData,MAX(o.OrderDate) as LastOrder FROM Customers c left join Orders o on c.CustomerID = o.CustomerID  group by c.CustomerName having count(*) >=5 order by MAX(o.OrderDate) desc;

## JavaScript/TypeScript Questions
1. Title case
```
function titleCase(str) {
  const words = str.split(" ");
  temp = []
  for (word of words){
    temp.push(word.charAt(0).toUpperCase()+ word.slice(1).toLowerCase())
  }
  const result = temp.join(" ");
  return result;
}
console.log(titleCase("sHoRt AnD sToUt"))
```

2. 
```
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

delay(3000).then(() => alert('runs after 3 seconds'));
```
2.5. 
```
function fetchData(url) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (!url) {
        reject("URL is required");
      } else {
        resolve(`Data from ${url}`);
      }
    }, 1000);
  });
}

function processData(data) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (!data) {
        reject("Data is required");
      } else {
        resolve(data.toUpperCase());
      }
    }, 1000);
  });
}

async function getData() {
  try {
    const data = await fetchData("https://example.com");
    const processedData = await processData(data);
    console.log("Processed Data:", processedData);
  } catch (error) {
    console.error("Error:", error);
  }
}

getData();
```

3. Create a real-time chat between two windows; using web sockets, vuejs and typescript.  
websocket: https://github.com/mivanrm/chatapp-backend
frontend : https://github.com/mivanrm/chatapp


# Vue.js
1. Explain Vue.js reactivity and common issues when tracking changes.\
    Vue reactivity is tracking between the data and the UI that currently rendered, if there are changes to the reactive data, vue will re render only the component that depend on the data. 
2. Describe data flow between components in a Vue.js app\
    Data in Vue flows from parent components down to child components through props. Child component to parent component through Events
3. List the most common cause of memory leaks in Vue.js apps and how they can be solved.\
    maybe not removing/ closing, event and object, such as interval and websocket connections. the solution is add the close/clear when component unmounted
4. What have you used for state management\
    only small experience with redux, and learning about pinia
5. What’s the difference between pre-rendering and server side rendering?\
    pre rendering generate static html at build time, meanwhile server side rendering generate html on each request. pre rendering better would atic content, server side rendering better for dynamic content. pre rendering will be load faster than server side rendering.

## Website Security Best Practises
1. use https for all the request, it is ensuring that all the data is encrpyted
2. validate and sanitize input, with validation and sanitize, we can avoid the possibility of attack by injection
3. use proper authentication system, use secure hashing on user password, limiting failed attempts, and implement two-factor authentication
4. use proper authorization, implement RBAC and validate permission on request
5. constantly updating packgae to avoid security issue
6. constanly check and monitor the logging and metrics of server


## Website Performance Best Practises
1. uses cdn to improve load time and reduce latency
2. improve request response time
- analyze and optimize database query call
- use cache for frequent data access
- use asynchronous task for long running process

3. compress asset to reduce file size


## Golang
```
package main

import (
	"fmt"
	"regexp"
	"strings"
)

func countword(s string) map[string]int {
	re := regexp.MustCompile(`[^\w\s]`)
	cleanedString := re.ReplaceAllString(s, "")
	words := strings.Fields(cleanedString)

	counts := make(map[string]int)
	for _, word := range words {
		word = strings.ToLower(word)
		counts[word]++
	}

	return counts
}

func main() {
	test := "Four, One two two three Three three four  four   four"
	ans := countword(test)
	for k := range ans {
		fmt.Printf("%s => %d \n", k, ans[k])
	}
}

```
## Tools
Git: 5
Redis:4
VSCode:4
Linux:4
AWS
-EC2:4
-Lambda:3
-RDS:3
-Cloudwatch:3
-S3:3
Unit testing:5
Kanban boards4
