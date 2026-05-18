# AWS Shape Reference for draw.io

All AWS shapes use the `mxgraph.aws4` icon library.

## CORE RULE — SURGICAL SUBSTITUTION

Never redesign a diagram's layout to add AWS components. The correct approach:

- Keep all swimlanes, zones, node positions, edges, and edge labels exactly as they are
- **Only** replace boxes that map to an AWS service with a 60×60 icon at the same centre position
- Non-AWS nodes (custom microservices, proxies, external systems) stay as plain rectangles
- Icon label sits to the **right** of the icon (`labelPosition=right;verticalLabelPosition=middle`) — keeps top/bottom edge connectors clear. Include service name + sub-details as two lines

---

## TOP-30 SERVICE CHEAT SHEET

Use `shape=mxgraph.aws4.<shape>` with the listed `fillColor`.

╔═══════════════════╦══════════════════════════════════════════════╦═══════════════╗
║ Common name       ║ shape= value                                 ║ fillColor     ║
╠═══════════════════╬══════════════════════════════════════════════╬═══════════════╣
║ EC2               ║ ec2                                          ║ #ED7100       ║
║ Lambda            ║ lambda                                       ║ #ED7100       ║
║ ECS               ║ elastic_container_service                    ║ #ED7100       ║
║ EKS               ║ elastic_kubernetes_service                   ║ #ED7100       ║
║ Fargate           ║ fargate                                      ║ #ED7100       ║
╠═══════════════════╬══════════════════════════════════════════════╬═══════════════╣
║ S3 ⚠              ║ resourceIcon;resIcon=mxgraph.aws4.s3         ║ #7AA116       ║
║ EBS               ║ elastic_block_store_ebs                      ║ #7AA116       ║
║ EFS               ║ elastic_file_system_efs                      ║ #7AA116       ║
╠═══════════════════╬══════════════════════════════════════════════╬═══════════════╣
║ RDS               ║ rds                                          ║ #C925D1       ║
║ DynamoDB          ║ dynamodb                                     ║ #C925D1       ║
║ ElastiCache       ║ elasticache                                  ║ #C925D1       ║
║ Redshift          ║ redshift                                     ║ #C925D1       ║
║ Aurora            ║ aurora                                       ║ #C925D1       ║
╠═══════════════════╬══════════════════════════════════════════════╬═══════════════╣
║ ELB / ALB         ║ elastic_load_balancing                       ║ #8C4FFF       ║
║ Route 53          ║ route_53                                     ║ #8C4FFF       ║
║ CloudFront        ║ cloudfront                                   ║ #8C4FFF       ║
║ API Gateway       ║ api_gateway                                  ║ #8C4FFF       ║
║ NAT Gateway       ║ nat_gateway                                  ║ #8C4FFF       ║
╠═══════════════════╬══════════════════════════════════════════════╬═══════════════╣
║ SQS               ║ sqs                                          ║ #E7157B       ║
║ SNS               ║ sns                                          ║ #E7157B       ║
║ EventBridge       ║ eventbridge                                  ║ #E7157B       ║
║ Kinesis           ║ kinesis                                      ║ #E7157B       ║
║ Step Functions    ║ step_functions                               ║ #E7157B       ║
╠═══════════════════╬══════════════════════════════════════════════╬═══════════════╣
║ IAM               ║ iam                                          ║ #DD344C       ║
║ Cognito           ║ cognito                                      ║ #DD344C       ║
║ KMS               ║ kms                                          ║ #DD344C       ║
╠═══════════════════╬══════════════════════════════════════════════╬═══════════════╣
║ CloudWatch        ║ cloudwatch                                   ║ #E7157B       ║
║ CloudTrail        ║ cloudtrail                                   ║ #E7157B       ║
╠═══════════════════╬══════════════════════════════════════════════╬═══════════════╣
║ Glue              ║ glue                                         ║ #8C4FFF       ║
║ Athena            ║ athena                                       ║ #8C4FFF       ║
╚═══════════════════╩══════════════════════════════════════════════╩═══════════════╝

⚠ **S3 exception**: `shape=mxgraph.aws4.simple_storage_service_s3` renders as a plain coloured box.
Always use `shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.s3` for S3.

**Category colours:**
- Compute (orange): `#ED7100`
- Storage (green): `#7AA116`
- Database (purple): `#C925D1`
- Networking (violet): `#8C4FFF`
- Messaging / App Integration (pink): `#E7157B`
- Security / Identity (red): `#DD344C`
