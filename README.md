Piinguin lambda relay
=====================

Pushes PII events to Piinguin using the piinguin client. 

This relay is meant to listen to EnrichedEvent formatted messages coming on an AWS Kinesis stream containing all the extracted PII from Snowplow. It then forwards those to a `piinguin-server`. Please read the piinguin project documentation for more information [here][piinguin].

Deployment
==========

You can always use the bintray version [here][bintray].

If you need to rebuild and deploy yourself then follow those steps:

* `sbt assembly`
* Copy artifact to S3 (it's too large for the console) by e.g. `aws s3 cp target/scala-2.12/piinguin-relay-assembly-0.1.0-SNAPSHOT.jar s3://somebucket/`

Configuration
=============

* Ensure that you are running this lambda in the same VPC and subnet as the piinguin server
* Ensure that the lambda is in a security group that is allowed to talk to the piinguin server
* Create a trigger from the kinesis stream that has the PII events coming from stream enrich
* Set the environment variables required by the lambda in the AWS console or the cli e.g.:
```
PIINGUIN_HOST = ec2-1-2-3-4.eu-west-1.compute.amazonaws.com
PIINGUIN_PORT = 8080
PIINGUIN_TIMEOUT_SEC = 10 (should be lower than the lambda timeout)
```
* Use the `Java 8` runtime and:
* * Handler should be: `com.snowplowanalytics.piinguin.relay.PiinguinRelay::recordHandler`
* * S3 URL link should be something like (depending on where you uploaded it): `https://s3-eu-west-1.amazonaws.com/somebucket/piinguin-relay-assembly-0.1.0-SNAPSHOT.jar`


## Copyright and license

Copyright 2012-2018 Snowplow Analytics Ltd.

Licensed under the [Apache License, Version 2.0][license] (the "License");
you may not use this software except in compliance with the License.

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[license]: http://www.apache.org/licenses/LICENSE-2.0
[piinguin]: https://github.com/snowplow-incubator/piinguin
[bintray]: https://bintray.com/snowplow/snowplow-generic/snowplow-piinguin-relay
