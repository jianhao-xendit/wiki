#   Chapter 1.8: Alerting
>This chapter explains how to install ElastAlert on your `Kali linux machine` and how to generate alerts to Slack and Cortex/The Hive, the latter is a ticketing system and SOAR solution.


|   |   |
|---|---|
| ![Screenshot ElastAlert](./assets/00-elastalert-sigma.png) | ElastAlert is a simple framework for alerting on anomalies, spikes, or other patterns of interest from data in Elasticsearch.  |
|   |   |

_"ElastAlert works by combining Elasticsearch with two types of components, rule types and alerts. Elasticsearch is periodically queried and the data is passed to the rule type, which determines when a match is found. When a match occurs, it is given to one or more alerts, which take action based on the match. ElastAlert is developed and opensourced by YELP"_

