# API 规范原则
首先，助教们作为产品经理，是按照功能规范进行 API 的定义。本 API 规范设计的初心在于实现 MVP（最小化可行产品），并方便项目验收时的测试程序编写。

同学们在参照API 规范进行开发的时，不必【完全遵守】我们的API规范，可以对 request 和 response 的参数进行【合理增加】。但是需要注意，项目后端需要至少保证【按照 API 文档的规范进行请求】时可以正常运行，不应当因为增加功能而导致原本的 API 规范【不可用】。

开放此 Github 仓库的目的是为了保证助教们（产品经理们）设计的 API 不会过于阻碍同学们的开发，同学们在开发过程中如何有认为 API 设计不合理的地方，可以提出 Issue 并说明修改原因，助教们会进行讨论并修改。

因此，API 文档可能会实时更新，请同学们按照最新的文档进行开发。

# 修改原则
1. 对于所有的 object 类型的对象，可以添加任意属性。因此为了保证最大程度上的可扩展性，API 文档将所有的 request 和 response 中的 data 字段都设置为了 object 类型。
2. 对于对象的类型，string、int、bool 等基础属性可以根据需要进行更改

# 验收步骤
在项目的验收阶段，我们会依照 API 文档编写 client，client 会向所有后端按照 request 规范发送测试请求，并按照 response 规范检查收到的包内容。因此，同学们可以增加任何功能，但至少保证 client 发送的 request 可以收到至少包含 response 规定的数据。（可多不可少）
举个例子，如果 register 接口的 request 和 response 请求规范如下：
```
request: {
    username: string,
    password: string
}
response: {
    code: integer,
    error_msg: string,
    data: {
        username: string
    }
}
```
同学们可以在注册时添加邮箱，因此 register 实际的业务请求可以表现为如下形式：
```
request: {
    username: string,
    password: string,
++  email: string
}
response: {
    code: integer,
    error_msg: string,
    data: {
        username: string,
++      email: string
    },
}
```
但是，如下行为会被可能会在验收时存在问题
(1) response 没有返回必要的字段
```
request: {
    username: string,
    password: string,
++  email: string
}
response: {
    code: integer,
    error_msg: string,
    data: {
        # 这里没有返回 username
++      email: string
    },
}
```
(2) 不能正确处理缺少字段的 request
```
request: {
    username: string,
    password: string,
}
response: {
    code: 400,
    # 无法正确处理不包含 email 的字段
    error_msg: "No email field in request",
    data: {},
}
```
# 提出 API 修改建议
按照如下步骤创建 issue 即可