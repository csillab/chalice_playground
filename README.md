# Chalice

Chalice is a tool to facilitate Python based lambda deployment. This repo contains the output of my basic exploration of this tool.

My specific goal with this tiny project was to examine the output of the terraform code and understand what resources will be created for a helloworld project.

I followed (this tutorial| https://aws.github.io/chalice/topics/tf.html), although used virtualenv a bit differently.

```
python3 --version
python3 -m venv venv38
. venv38/bin/activate
```

As instructed in the tutorial, instead of `chalice deploy`, I used `chalice package` to specify terraform as the deployment method. The output folder contained the zip file for the lambda and the terraform file in JSON format. 
`terraform` should be run in the output folder.

```
chalice package --pkg-format terraform /tmp/packaged-app/
```

# Terraform output

Output of the `terraform apply` command shows the creation of the following resources:
 - aws_api_gateway_deployment
 - aws_api_gateway_rest_api
 - aws_iam_role
 - aws_iam_role_policy
 - aws_lambda_function
 - aws_lambda_permission

```terraform

(venv38) csillabessenyei@Csillas-MacBook-Air packaged-app % terraform apply
provider.aws.region
  The region where AWS operations will take place. Examples
  are us-east-1, us-west-2, etc.

  Enter a value: eu-west-1


Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_api_gateway_deployment.rest_api will be created
  + resource "aws_api_gateway_deployment" "rest_api" {
      + created_date      = (known after apply)
      + execution_arn     = (known after apply)
      + id                = (known after apply)
      + invoke_url        = (known after apply)
      + rest_api_id       = (known after apply)
      + stage_description = (known after apply)
      + stage_name        = "api"
    }

  # aws_api_gateway_rest_api.rest_api will be created
  + resource "aws_api_gateway_rest_api" "rest_api" {
      + api_key_source               = (known after apply)
      + arn                          = (known after apply)
      + binary_media_types           = [
          + "application/octet-stream",
          + "application/x-tar",
          + "application/zip",
          + "audio/basic",
          + "audio/ogg",
          + "audio/mp4",
          + "audio/mpeg",
          + "audio/wav",
          + "audio/webm",
          + "image/png",
          + "image/jpg",
          + "image/jpeg",
          + "image/gif",
          + "video/ogg",
          + "video/mpeg",
          + "video/webm",
        ]
      + body                         = (known after apply)
      + created_date                 = (known after apply)
      + description                  = (known after apply)
      + disable_execute_api_endpoint = (known after apply)
      + execution_arn                = (known after apply)
      + id                           = (known after apply)
      + minimum_compression_size     = -1
      + name                         = "test-tf-deploy"
      + policy                       = (known after apply)
      + root_resource_id             = (known after apply)
      + tags_all                     = (known after apply)

      + endpoint_configuration {
          + types            = [
              + "EDGE",
            ]
          + vpc_endpoint_ids = (known after apply)
        }
    }

  # aws_iam_role.default-role will be created
  + resource "aws_iam_role" "default-role" {
      + arn                   = (known after apply)
      + assume_role_policy    = jsonencode(
            {
              + Statement = [
                  + {
                      + Action    = "sts:AssumeRole"
                      + Effect    = "Allow"
                      + Principal = {
                          + Service = "lambda.amazonaws.com"
                        }
                      + Sid       = ""
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + create_date           = (known after apply)
      + force_detach_policies = false
      + id                    = (known after apply)
      + managed_policy_arns   = (known after apply)
      + max_session_duration  = 3600
      + name                  = "test-tf-deploy-dev"
      + name_prefix           = (known after apply)
      + path                  = "/"
      + tags_all              = (known after apply)
      + unique_id             = (known after apply)

      + inline_policy {
          + name   = (known after apply)
          + policy = (known after apply)
        }
    }

  # aws_iam_role_policy.default-role will be created
  + resource "aws_iam_role_policy" "default-role" {
      + id     = (known after apply)
      + name   = "default-rolePolicy"
      + policy = jsonencode(
            {
              + Statement = [
                  + {
                      + Action   = [
                          + "logs:CreateLogGroup",
                          + "logs:CreateLogStream",
                          + "logs:PutLogEvents",
                        ]
                      + Effect   = "Allow"
                      + Resource = "arn:*:logs:*:*:*"
                    },
                ]
              + Version   = "2012-10-17"
            }
        )
      + role   = (known after apply)
    }

  # aws_lambda_function.api_handler will be created
  + resource "aws_lambda_function" "api_handler" {
      + architectures                  = (known after apply)
      + arn                            = (known after apply)
      + filename                       = "./deployment.zip"
      + function_name                  = "test-tf-deploy-dev"
      + handler                        = "app.app"
      + id                             = (known after apply)
      + invoke_arn                     = (known after apply)
      + last_modified                  = (known after apply)
      + memory_size                    = 128
      + package_type                   = "Zip"
      + publish                        = false
      + qualified_arn                  = (known after apply)
      + reserved_concurrent_executions = -1
      + role                           = (known after apply)
      + runtime                        = "python3.8"
      + signing_job_arn                = (known after apply)
      + signing_profile_version_arn    = (known after apply)
      + source_code_hash               = "G/z6BLWCK/9iNDEl1lRyYpXyFoo/hJkW0NdhLEf5tUc="
      + source_code_size               = (known after apply)
      + tags                           = {
          + "aws-chalice" = "version=1.26.5:stage=dev:app=test-tf-deploy"
        }
      + tags_all                       = {
          + "aws-chalice" = "version=1.26.5:stage=dev:app=test-tf-deploy"
        }
      + timeout                        = 60
      + version                        = (known after apply)

      + tracing_config {
          + mode = (known after apply)
        }
    }

  # aws_lambda_permission.rest_api_invoke will be created
  + resource "aws_lambda_permission" "rest_api_invoke" {
      + action        = "lambda:InvokeFunction"
      + function_name = (known after apply)
      + id            = (known after apply)
      + principal     = "apigateway.amazonaws.com"
      + source_arn    = (known after apply)
      + statement_id  = (known after apply)
    }

Plan: 6 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + EndpointURL = (known after apply)
  + RestAPIId   = (known after apply)
╷
│ Warning: Deprecated Resource
│
│   with data.null_data_source.chalice,
│   on chalice.tf.json line 109, in data.null_data_source.chalice:
│  109:       }
│
│ The null_data_source was historically used to construct intermediate values to re-use elsewhere in configuration, the same can now be achieved using locals
│
│ (and one more similar warning elsewhere)
╵

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_iam_role.default-role: Creating...
aws_iam_role.default-role: Creation complete after 2s [id=test-tf-deploy-dev]
aws_iam_role_policy.default-role: Creating...
aws_lambda_function.api_handler: Creating...
aws_iam_role_policy.default-role: Creation complete after 1s [id=test-tf-deploy-dev:default-rolePolicy]
aws_lambda_function.api_handler: Still creating... [10s elapsed]
aws_lambda_function.api_handler: Creation complete after 14s [id=test-tf-deploy-dev]
aws_api_gateway_rest_api.rest_api: Creating...
aws_api_gateway_rest_api.rest_api: Creation complete after 1s [id=fzegk1qj6c]
aws_api_gateway_deployment.rest_api: Creating...
aws_lambda_permission.rest_api_invoke: Creating...
aws_lambda_permission.rest_api_invoke: Creation complete after 1s [id=terraform-20220203152421383500000001]
aws_api_gateway_deployment.rest_api: Creation complete after 1s [id=g8xs21]

Apply complete! Resources: 6 added, 0 changed, 0 destroyed.

```