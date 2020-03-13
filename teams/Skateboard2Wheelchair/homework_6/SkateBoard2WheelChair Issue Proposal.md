# SkateBoard2WheelChair Issue Proposal 

Last time, the maintainer of elasticsearch gave us some advice about what to work on as a beginner. We are not sure what kind of issue is suitble for the HW, so  following their advice, we now have two alternatives.

The first one is [Remove support for delaying state recovery pending master nodes #51806](https://github.com/elastic/elasticsearch/issues/51806). This issue described that it is useful to delay state recovery until enough data nodes are available. Currently we have the option to delay state recovery until a certain number of master-eligible nodes are available, but it seems unnecessay so they would like to deprecate this setting and remove in the future. The should include two PR, one is to deprecate the related settings, another is to remove them.

The second one is [Add null check for ParentJoinFieldMapper in ChildrenAggregationBuilder.joinFieldResolveConfig #42997](https://github.com/elastic/elasticsearch/issues/42997). This issue describes that ParentJoinFieldMapper.getMapper returns null when there is no parent-join field in the mapping, thus may result in NullPointerException. We should add null check to this field. Fixing this requires us to first fix this in the source code, and then write corresponding test case to ensure it works ok.

Could you tell us which one would be a better fit for this hw?