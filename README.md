# News Project

Single-page news website to output real-time news and it has search feature to find related news content.

![](https://media.giphy.com/media/Rb2irgUZXdnqFnHJRl/giphy.gif)

![](https://media.giphy.com/media/8rA50DUp86gucV01vN/giphy.gif)

## Backend
- Spring Boot v.2.4.4 https://mvnrepository.com/
- Maven v.3.6.3 https://maven.apache.org/
- Apache HttpClient v.4.5.8 https://mvnrepository.com/
- Yahoo Finance API https://rapidapi.com/apidojo/api/yahoo-finance1
- CNBC API https://rapidapi.com/apidojo/api/cnbc/


## Development
### Getting started
### Frontend
All necessary depedencies are stored in package.json.

I implemented event.preventDefault() to prevent default event call from occuring.

To install all dependencies: `npm install`

To run the client: `npm start`

### Backend

All necessary dependencies are stored in pom.xml.

To run the server: `mvn clean spring boot run`

## Project Overview

1. **Deciding the environment for date conversion:** Converted the date formats on the backend from various news platforms to a single uniform format and sorted the news articles by published date. In the front end I determined the display date which was a calculation of current date and time minus the publish date. I chose to do the display date conversion in the front end to eliminate an additional API call to reduce the cost and chance of an API failure.

2. **Creating an abstract class for RestConnector:** Implemented an abstract class, RestConnector, which contains commonly used methods and variables. RestConnector employs httpclient to communicate with the connectors and to operate CRUD actions. I extended the RestConnector into Connector classes. By utilizing @Value annotation, I injected values such as API Keys and the end point URL without exposing them in the field.

3. **Designing Search feature:** Rather than adding every possible feature to the RestConnector class, I used inheritance to define subclasses that extends to different Connectors. To minimize the number of API calls, I implemented an event handler on the frontend to render upon search submission.

4. **[Caching and TTL:](https://github.com/sungpark1/FeedMe/blob/main/src/main/java/project/newsfeed/connectors/YahooSearchConnector.java#L32, "Caching & TTL")** To further minimize the API calls, I implemented caching. My decision to add caching for the news search API solved the use case of a user entering the same search keyword. The cache will allow the user to retrieve the list of news directly from the in-memory storage, not from a separate API call. One other consideration was to implement TTL (time to live) which sets the timeframe that the cache is active. This is important since users want the latest news updates and cache will only store the articles that are presented on the first search action that the user takes within the set timeframe. After the cache expires the results will be removed and a new search action will be taken to retrieve the latest news.

