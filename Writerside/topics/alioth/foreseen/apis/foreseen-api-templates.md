# 远焦模板接口

## 关联数据库文档

1. [模板信息表](foreseen-database-templates.md)

## 详细接口文档

<api-doc openapi-path="./foreseen.yaml" tag="templates">
    <api-endpoint endpoint="/template/${template_name}" method="GET">
        <response type="200"><sample lang="JSON" title="success">
            {
                "error_code": 0,
                "error_message": "success",
                "data": {
                    "template_id": 114514,
                    "name": "example_template",
                    "content": "${project} now is ${status} at ${hour}:${min}",
                    "arguments": {
                        "project": "project_name",
                        "status": "offline",
                        "hour": 12,
                        "min": 30
                    },
                    "preview": "project_name now is offline at 12:30"
                }
            }
        </sample></response>
        <response type="401"><sample lang="JSON" title="authorization error">
            {
                "error_code": 4011,
                "error_message": "invalid authorization token"
            }
        </sample></response>
        <response type="404"><sample lang="JSON" title="template not found">
            {
                "error_code": 4041,
                "error_message": "template not exist"
            }
        </sample></response>
    </api-endpoint>
    <api-endpoint endpoint="/template" method="POST">
        <request>
            <sample lang="JSON" title="create template with arguments">
                {
                    "name": "example_template",
                    "content": "${project} now is ${status} at ${hour}:${min}",
                    "arguments": {
                        "project": "project_name",
                        "status": "offline",
                        "hour": 12,
                        "min": 30
                    }
                }
            </sample>
        </request>
        <response type="200"><sample lang="JSON" title="success">
            {
                "name": "example_template",
                "content": "${project} now is ${status} at ${hour}:${min}",
                "arguments": {
                    "project": "project_name",
                    "status": "offline",
                    "hour": 12,
                    "min": 30
                }
            }
        </sample></response>
        <response type="401"><sample lang="JSON" title="authorization error">
            {
                "error_code": 4011,
                "error_message": "invalid authorization token"
            }
        </sample></response>
        <response type="409"><sample lang="JSON" title="template exist">
            {
                "error_code": 4091,
                "error_message": "template already exist"
            }
        </sample></response>
    </api-endpoint>
    <api-endpoint endpoint="/template/${template_name}" method="PUT">
        <request>
            <sample lang="JSON" title="update all template">
                {
                    "content": "${project} now is ${status} at ${hour}:${min}",
                    "arguments": {
                        "project": "project_name",
                        "status": "offline",
                        "hour": 12,
                        "min": 30
                    }
                }
            </sample>
            <sample lang="JSON" title="update template partly">
                {
                    "content": "${project} now is ${status} at ${hour}:${min}"
                }
            </sample>
        </request>
        <response type="200"><sample lang="JSON" title="success">
            {
                "error_code": 0,
                "error_message": "success",
                "data": {
                    "template_id": 114514,
                    "name": "example_template",
                    "content": "${project} now is ${status} at ${hour}:${min}",
                    "arguments": {
                        "project": "project_name",
                        "status": "offline",
                        "hour": 12,
                        "min": 30
                    },
                    "preview": "project_name now is offline at 12:30"
                }
            }
        </sample></response>
        <response type="401"><sample lang="JSON" title="authorization error">
            {
                "error_code": 4011,
                "error_message": "invalid authorization token"
            }
        </sample></response>
        <response type="404"><sample lang="JSON" title="template not found">
            {
                "error_code": 4041,
                "error_message": "template not exist"
            }
        </sample></response>
    </api-endpoint>
    <api-endpoint endpoint="/template/${template_name}" method="DELETE">
        <response type="200"><sample lang="JSON" title="success">
            {
                "error_code": 0,
                "error_message": "success"
            }
        </sample></response>
        <response type="401"><sample lang="JSON" title="authorization error">
            {
                "error_code": 4011,
                "error_message": "invalid authorization token"
            }
        </sample></response>
        <response type="404"><sample lang="JSON" title="template not found">
            {
                "error_code": 4041,
                "error_message": "template not exist"
            }
        </sample></response>
    </api-endpoint>
    <api-endpoint endpoint="/template/${template_name}/preview" method="GET">
        <request>
            <sample lang="JSON" title="preview with arguments">
                {
                    "project": "project_name",
                    "status": "offline",
                    "hour": 12,
                    "min": 30
                }
            </sample>
            <sample lang="JSON" title="preview without arguments">
                null
            </sample>
        </request>
        <response type="200"><sample lang="JSON" title="success">
            {
                "error_code": 0,
                "error_message": "success",
                "data": {
                    "preview": "project_name now is offline at 12:30"
                }
            }
        </sample></response>
        <response type="401"><sample lang="JSON" title="authorization error">
            {
                "error_code": 4011,
                "error_message": "invalid authorization token"
            }
        </sample></response>
        <response type="404"><sample lang="JSON" title="template not found">
            {
                "error_code": 4041,
                "error_message": "template not exist"
            }
        </sample></response>
    </api-endpoint>
</api-doc>