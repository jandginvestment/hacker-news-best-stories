# Hacker News Best Stories Restful API Service
## Table of contents
- [ ] [Overview](#Overview)
  - [ ] [Features](#Features)
  - [ ] [Assumptions](#Assumptions)
- [ ] [Technical Overview](#Technical-Overview)
  - [ ] [Key Components](#Key-Components)
  - [ ] [Data Flow](#Data-Flow)
  - [ ] [Technology Used](#Technology-used)
  - [ ] [Installation and Setup of the Project](#Installation-and-Setup-of-the-project)
  - [ ] [Usage / Code Structure](#Usage-/-Code-Structure)
    - [ ] [API EndPoint:](#API-EndPoint:)
    - [ ] [HackerNewsService](#HackerNewsService)
- [ ] [Benefits](#Benefits)
- [ ] [Future Enhancements](#Future-Enhancements)
<hr style="border-top: 1px solid;"/>
## Overview
 The `HNBS` is a .NET core minimal webAPI service that leverages the Hacker News API to fetch the best "n" number of stories
### Features
 - Retrieves the "n" number of the best hacker news stories based on the score. [n limits to 200]
 - Caches the `best hacker news stories` in Azure Redis  to improve the performance and efficiently handle large numbers of requests
without the risk of overloading the Hacker News API.

> Azure Redis is a centralized and distributable cache service that provides a highly available and scalable solution for storing and retrieving data in-memory, facilitating efficient and fast access to frequently accessed data across applications and services
### Assumptions
*Our investigation revealed that no official endpoints exist to subscribe to "Score" changes within the "best hacker news" section. Based on this finding, we can reasonably assume that scores refresh on a 5-second interval.*
## Technical Overview
 - ### **Key Components**
	-  `HackerNewsService` : **Retrieves and caches `the best hacker news stories`**
	- `IHttpClientFactory` : Manages HTTP clients to access the data from the uri [beststories](https://hacker-news.firebaseio.com/v0/beststories.json)
	- `StackExchange.Redis`: Manages **Azure Redis** for caching the retrived data.
- ### **Data Flow**
1. HttpClient requests best hacker news stories via API endpoints.
1. `HackerNewsService` checks for cached stories from `Azure Redis`
1. If stories are cached, the data returned as an immediate effect.
1. If not caced, `best hacker news stories` are fetched.  

### **Technology Used**
- C#
- asp.Net core 8.0 minimal API
- httpClientFactory
- StackExchange.Redis
- Newtonsoft.Json
### Installation and Setup of the Project
1. Installed the required dependancies using nuget package.
1. The `Azure Redis` connection string is configured (in appsettings.json).
1. can be packaged and deployable in multiple environments.

### Usage / Code Structure
 #### API EndPoint:
  - ``GET`` : {localhost:port}/api/hnbs?numberOfStories = \{number}
	 > this method returns \{number} of JSON array of `HackerNewsStoryDTO` objects.
#### HackerNewsService
   -  `GetHackerNewsBestStories`: Retrieves best story based on the scores, fresh or cached.
   -  `GetCachedStoriesAsync`: Asynchronously retrieves the cached stories from Azure Redis. 
   -  `SetCachedStoriesAsync`: Caches the retrieved  stories from GetHackerNewsBestStories.
## Benefits

-  Efficiently fetches and caches top Hacker News stories.
-  Minimizes API calls and reduces server load.
-  Provides a simple API for easy integration into applications.
-  Leverages Azure Redis for scalable caching.

## Future Enhancements
- unit test cases and load/stress test cases needs to be added. 
