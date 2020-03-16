# SkateBoard2WheelChair Issue Proposal 

Last time, the maintainer of elasticsearch gave us some advice about what to work on as a beginner. 

The issue we plan to work on is [Remove support for delaying state recovery pending master nodes #51806](https://github.com/elastic/elasticsearch/issues/51806). This issue described that it is useful to delay state recovery until enough data nodes are available. Currently we have the option to delay state recovery until a certain number of master-eligible nodes are available, but it seems unnecessay so they would like to deprecate this setting and remove in the future. The should include two PR, one is to deprecate the related settings, another is to remove them.

