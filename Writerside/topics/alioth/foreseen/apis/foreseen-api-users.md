# 远焦用户接口

## 关联数据库文档

1. [用户信息表](foreseen-database-users.md)
2. [用户被通知账户信息表](foreseen-database-accounts.md)

## 详细接口文档

<api-doc openapi-path="./foreseen.yaml">
    <api-endpoint endpoint="/user/${username}" method="GET">
        <response type="200"><sample lang="JSON" title="success">
            {
                "error_code": 0,
                "error_message": "success",
                "data": {
                    "user_id": 114514,
                    "username": "example_user",
                    "usertype": "user",
                    "accounts": [
                        {
                            "account_id": 1919810,
                            "integration": "knock",
                            "account": "GTR_1145141919810"
                        }
                    ]
                }
            }
        </sample></response>
        <response type="401"><sample lang="JSON" title="authorization error">
            {
                "error_code": 4011,
                "error_message": "invalid authorization token"
            }
        </sample> </response>
        <response type="404"><sample lang="JSON" title="user not found">
            {
                "error_code": 4041,
                "error_message": "user not exist"
            }
        </sample></response>
    </api-endpoint>
    <api-endpoint endpoint="/user" method="POST">
        <request>
            <sample lang="JSON" title="create user with account">
                {
                    "username": "example_user",
                    "usertype": "user",
                    "accounts": [
                        {
                            "integration": "lark",
                            "account_id": "ou_1145141919810"
                        }
                    ]
                }
            </sample>
            <sample lang="JSON" title="create user only">
                {
                    "username": "example_user",
                    "usertype": "user"
                }
            </sample>
        </request>
        <response type="200"><sample lang="JSON" title="success">
                {
                    "error_code": 0,
                    "error_message": "success",
                    "data": {
                        "user_id": 114514,
                        "username": "example_user",
                        "usertype": "user",
                        "accounts": [
                            {
                                "account_id": 1919810,
                                "integration": "lark",
                                "account": "ou_1145141919810"
                            }
                        ]
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
                        "error_message": "integration not found"
                    }
            </sample></response>
            <response type="409"><sample lang="JSON" title="user exist">
                    {
                        "error_code": 4092,
                        "error_message": "user already exist"
                    }
            </sample></response>
            <response type="409"><sample lang="JSON" title="account exist">
                    {
                        "error_code": 4093,
                        "error_message": "account already exist"
                    }
            </sample></response>
    </api-endpoint>
    <api-endpoint endpoint="/user/${username}" method="PUT">
        <request>
            <sample lang="JSON" title="single type actions">
                [
                    {
                        "action": "create",
                        "integration": "lark",
                        "account_id": "ou_1145141919810"
                    },
                    {
                        "action": "create",
                        "integration": "knock"
                        "account_id": "GTR_1145141919810"
                    }
                ]
            </sample>
            <sample lang="JSON" title="mixed type actions">
                [
                    {
                        "action": "delete",
                        "integration": "slack"
                    }
                    {
                        "action": "update",
                        "integration": "dingtalk",
                        "account_id": "1145141919810"
                    },
                    {
                        "action": "create",
                        "integration": "youtrack",
                        "account_id": "1145141919810"
                    }
                ]
            </sample>
        </request>
        <response type="200"><sample lang="JSON" title="success">
            {
                "username": "example_user",
                "usertype": "user",
                "accounts": [
                    {
                        "integration": "lark",
                        "account_id": "ou_1145141919810"
                    }
                ]
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
                "error_message": "integration not found"
            }
        </sample></response>
        <response type="409"><sample lang="JSON" title="user exist">
            {
                "error_code": 4092,
                "error_message": "user already exist"
            }
        </sample></response>
        <response type="409"><sample lang="JSON" title="account exist">
            {
                "error_code": 4093,
                "error_message": "account already exist"
            }
        </sample></response>
    </api-endpoint>
</api-doc>
