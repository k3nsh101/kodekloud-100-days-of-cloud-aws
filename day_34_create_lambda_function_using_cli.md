### Task

The Nautilus DevOps team continues to explore serverless architecture by setting up another Lambda function. This time, the task must be completed using the AWS Console to familiarize the team with the web interface. The function will return a custom greeting and demonstrate the capabilities of AWS Lambda effectively.

1. **Create Python Script:** Create a Python script named `lambda_function.py` with a function that returns the body `Welcome to KKE AWS Labs!` and status code `200`.
2. **Zip the Python Script:** Zip the script into a file named `function.zip`.
3. **Create Lambda Function:** Create a Lambda function named `nautilus-lambda-cli` using the zipped file and specify `Python` as the runtime.
4. **IAM Role:** Use the IAM role named `lambda_execution_role`.

Use AWS CLI which is already configured on the `aws-client` host.

### Solution

- Create lambda function file

  ```python
  def lambda_handler(event, context):
      return {
          "statusCode": 200,
          "body": "Welcome to KKE AWS Labs!"
      }
  ```

- Zip the file

  ```bash
  zip function.zip lambda_function.py
  ```

- Get aws account id. Needed for the role arn

  ```bash
  aws sts get-caller-identity
  ```

- Create lambda function. Replace the `<account_id>` with the account_id got from the above output.

  ```bash
  aws lambda create-function \
      --function-name nautilus-lambda-cli \
      --runtime python3.14 \
      --role arn:aws:iam::<account_id>:role/lambda_execution_role \
      --handler lambda_function.lambda_handler \
      --package-type Zip
      --zip-file fileb://function.zip
  ```

  `fileb` is used to specify the binary files like zips, images

- Verify the function is created successfully

  ```bash
  aws lambda get-function --function-name nautilus-lambda-cli
  ```

- Invoke and verify the lambda function is running successfully

  ```bash
  aws lambda invoke --function-name xfusion-lambda-cli output.json
  ```

- For detailed output view the content of `output.json`

  ```bash
  cat output.json
  ```

  The output should be as following

  ```
  {"statusCode": 200, "body": "Welcome to KKE AWS Labs!"}
  ```
