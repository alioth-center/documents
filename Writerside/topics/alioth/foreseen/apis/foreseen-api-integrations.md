# 远焦集成接口

## 关联数据库文档

1. [](foreseen-database-integrations.md)

## 详细接口文档

<api-doc openapi-path="./foreseen.yaml" tag="integrations">
    <api-endpoint endpoint="/integration/${integration_name}" method="GET">
        <response type="200"><sample lang="JSON" title="success">
            {
                "error_code": 0,
                "error_message": "success",
                "data": {
                    "integration_id": 114514,
                    "integration_name": "custom_integration"
                }
            }
        </sample></response>
        <response type="401"><sample lang="JSON" title="authorization error">
            {
                "error_code": 4011,
                "error_message": "invalid authorization token"
            }
        </sample></response>
        <response type="404"><sample lang="JSON" title="integration not found">
            {
                "error_code": 4041,
                "error_message": "integration not exist"
            }
        </sample></response>
    </api-endpoint>
    <api-endpoint endpoint="/integration" method="POST">
        <request>
            <sample lang="JSON" title="create integration">
                {
                    "name": "lark",
                    "secrets": [
                        "app_id",
                        "app_token",
                        "app_secret",
                        "return_url"
                    ]
                }
            </sample>
        </request>
        <response type="200"><sample lang="JSON" title="create success">
            {
                "error_code": 0,
                "error_message": "success",
                "data": {
                    "integration_id": 114514,
                    "integration_name": "custom_integration"
                }
            }
        </sample></response>
        <response type="401"><sample lang="JSON" title="authorization error">
            {
                "error_code": 4011,
                "error_message": "invalid authorization token"
            }
        </sample></response>
        <response type="409"><sample lang="JSON" title="integration exist">
            {
                "error_code": 4091,
                "error_message": "integration already exist"
            }
        </sample></response>
    </api-endpoint>
</api-doc>