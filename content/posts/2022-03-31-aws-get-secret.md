---
title: aws-get-secret
author: hbanken
type: post
date: 2022-03-31T23:33:00+02:00
url: /2022/03/31/aws-get-secret/
categories:
  - cloudnative
  - aws
  - serverless

---

Coming from a Google (GCP) ecosystem, there are many things that AWS does not do, that you'll miss. One of these things
if automatically [mounting secrets from Secret Manager](https://cloud.google.com/run/docs/configuring/secrets) on your
serverless Cloud Run instances like so:

```bash
gcloud run deploy SERVICE --update-secrets=PATH=SECRET_NAME:VERSION
```

Unfortunately, this is not possible with AWS Lambda, although it would not be a big of a leap for AWS to add this
feature somewhere soon. Instead, the typical way to retrieve to retrieve a secret with AWS Lambda today would be to
retrieve it at [runtime](https://dev.to/aws-builders/how-to-use-secrete-manager-in-aws-lambda-node-js-3j80):

```typescript
async function getSecret(secretName, region){
  const config = { region : region }
  var secret, decodedBinarySecret;
  let secretsManager = new AWS.SecretsManager(config);
  let secretValue = await secretsManager.getSecretValue({SecretId: secretName}).promise();
  if ('SecretString' in secretValue) {
    return secretValue.SecretString;
  } else {
    let buff = new Buffer(secretValue.SecretBinary, 'base64');
    return buff.toString('ascii');
  }
}
```

Before Google added Secret Manager support to Cloud Run, there was another utility that I often found myself using:
[gcp-get-secret](https://github.com/binxio/gcp-get-secret). This could be used like the snippet below, and the tool
would invoke a next command with an environment variable containing the replaced secret value.

```bash
export MYSQL_PASSWORD=gcp:///mysql_root_password'
gcp-get-secret bash -c 'echo $MYSQL_PASSWORD'
```

Such a tool did not exist for AWS yet as far as I know, until today, as I just published [aws-get-secret](https://github.com/hermanbanken/aws-get-secret).
This tool works much the same like `gcp-get-secret` but instead uses the prefix `aws:///` and understands ARN (or the shorter secret name).

For example:
```bash
export MYSQL_PASSWORD=aws:///mysql_root_password'
aws-get-secret bash -c 'echo $MYSQL_PASSWORD'
```

While this is convenient if you control your full image (like when running ECR images on Lambdas), it can also be
used with other runtimes, like NodeJS or Python, by using a Lambda Layer and the `AWS_LAMBDA_EXEC_WRAPPER` environment
variable. If your Lambda sets this value to `AWS_LAMBDA_EXEC_WRAPPER=/opt/aws-get-secret` then this wraps the actual
execution of the Lambda with `aws-get-secret` and the real handler has the real secret values available in its
environment.

The layer is also published as NodeJS package, allowing you to integrate it with CDK and/or serverless-stack:

```typescript
import { wrapLambdasWithSecrets } from "aws-get-secret-lambda"

export class SomeStack extends Stack {
  constructor(scope: Construct) {
    super(scope, 'SomeStack');
    this.setDefaultFunctionProps({
        timeout: 20,
        memorySize: 512,
        environment: { MYSQL_PASSWORD: "aws:///mysql_root_password" },
    });
    // Create the HTTP API
    const api = new sst.Api(this, "Api", {
      routes: {
        "GET /notes": "src/list.main",
        "GET /notes/{id}": "src/get.main",
        "PUT /notes/{id}": "src/update.main",
      },
    });
    // Load any configured secrets in all Lambdas
    wrapLambdasWithSecrets(this.getAllFunctions());
  }
}
```

Try it today, and let me know if it helped you or if you have any feedback! Reach out to me at [Twitter](https://twitter.com/hermanbanken).
