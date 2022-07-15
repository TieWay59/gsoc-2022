# Coding Period 1 week-05

## The Gitee Enriched Data

Like the GitHub API[^1], Gitee also has documented their data API [^2](specifically using waggger).Although not as detailed, it roughly allows me to deduce the meaning.

![data-search-screenshot](assets/01-data-search-screenshot.png)

workflow goes perceval backend -> elastic search -> enricher -> elastic search -> metric model 'study' module -> elastic search -> metric model algorithm -> output/kibana? (conversion rate requires a custom formula that is not possible on Kibana I think) (thanks @Mable to inform me about it.)

WIP

[^1]: [Issue event types - GitHub Docs](https://docs.github.com/en/developers/webhooks-and-events/events/issue-event-types)
[^2]: [Gitee API 文档](https://gitee.com/api/v5/swagger#/getV5ReposOwnerRepoIssues)
