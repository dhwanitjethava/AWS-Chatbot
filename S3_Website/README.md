# Copyright 2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License"). You may not use this file except
# in compliance with the License. A copy of the License is located at
#
# https://aws.amazon.com/apache-2-0/
#
# or in the "license" file accompanying this file. This file is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the
# specific language governing permissions and limitations under the License.

# Static Website

The goal of this website is to support the learning of the Building Serverless Applications course.

This folder is designed to be downloaded locally and then uplaoded as is to S3, strictly following the instructions in the excercise guide.,


Thought the lab you may need to edit a config file. Simpy replace the `null` with with your content.

For example inside config.json you would replace

```JavaScript
		var G_API_GATEWAY_URL_STR = null;
```

with

```JavaScript

		var G_API_GATEWAY_URL_STR = "https://eois2fhvse.execute-api.us-east-1.amazonaws.com/test"
```
according to the excercise guide.


Then push that back up to the root of the website.

Eveything else you need to know about this excercise is in the excercise guide.