# 使用 Azure OpenAI 部署

LobeChat 支持使用 [Azure OpenAI][azure-openai-url] 作为 OpenAI 的模型服务商，本文将介绍如何配置 Azure OpenAI。

#### TOC

- [使用限制](#使用限制)
- [在界面中配置](#在界面中配置)
- [在部署时配置](#在部署时配置)

## 使用限制

从研发成本考虑 ([#178][rfc])，目前阶段的 LobeChat 并没有 100% 完全符合 Azure OpenAI 的实现模型，采用了以 `openai` 为基座，兼容 Azure OpeAI 的解决方案。因此会带来以下局限性：

- OpenAI 与 Azure OpenAI 只能二选一，当你开启使用 Azure OpenAI 后，将无法使用 OpenAI 作为模型服务商；
- LobeChat 约定了与模型同名的部署名才能正常使用，比如 `gpt-35-turbo` 模型的部署名，必须为 `gpt-35-turbo`，否则 LobeChat 将无法正常正确匹配到相应模型
  ![](https://github-production-user-asset-6210df.s3.amazonaws.com/28616219/267082091-d89d53d3-1c8c-40ca-ba15-0a9af2a79264.png)
- 由于 Azure OpenAI 的 SDK 接入复杂度，当前无法查询配置资源的模型列表；

## 在界面中配置

点击左下角「操作」 -「设置」，切到 「语言模型」 Tab 后通过开启「Azure OpenAI」开关，即可开启使用 Azure OpenAI。

![](https://github-production-user-asset-6210df.s3.amazonaws.com/28616219/267083420-422a3714-627e-4bef-9fbc-141a2a8ca916.png)

你按需填写相应的配置项：

- **APIKey**：你在 Azure OpenAI 账户页面申请的 API 密钥，可在 “密钥和终结点” 部分中找到此值
- **API 地址**：Azure API 地址，从 Azure 门户检查资源时，可在 “密钥和终结点” 部分中找到此值
- **Azure Api Version**： Azure 的 API 版本，遵循 YYYY-MM-DD 格式，查阅[最新版本][azure-api-verion-url]

完成上述字段配置后，点击「检查」，如果提示「检查通过」，则说明配置成功。

<br/>

## 在部署时配置

如果你希望部署的版本直接配置好 Azure OpenAI，让终端用户直接使用，那么你需要在部署时配置以下环境变量：

| 环境变量            | 类型 | 描述                                                                             | 默认值             | 示例                                               |
| ------------------- | ---- | -------------------------------------------------------------------------------- | ------------------ | -------------------------------------------------- |
| `USE_AZURE_OPENAI`  | 必选 | 设置改值为 `1` 开启 Azure OpenAI 配置                                            | -                  | `1`                                                |
| `AZURE_API_KEY`     | 必选 | 这是你在 Azure OpenAI 账户页面申请的 API 密钥                                    | -                  | `c55168be3874490ef0565d9779ecd5a6`                 |
| `OPENAI_PROXY_URL`  | 必选 | Azure API 地址，从 Azure 门户检查资源时，可在 “密钥和终结点” 部分中找到此值      | -                  | `https://docs-test-001.openai.azure.com`           |
| `AZURE_API_VERSION` | 可选 | Azure 的 API 版本，遵循 YYYY-MM-DD 格式                                          | 2023-08-01-preview | `2023-05-15`，查阅[最新版本][azure-api-verion-url] |
| `ACCESS_CODE`       | 可选 | 添加访问此服务的密码，你可以设置一个长密码以防被爆破，该值用逗号分隔时为密码数组 | -                  | `awCT74` 或 `e3@09!` or `code1,code2,code3`        |

> \[!NOTE]
>
> 当你在服务端开启 `USE_AZURE_OPENAI` 后，用户将无法在前端配置中修改并使用 OpenAI key。

[azure-api-verion-url]: https://learn.microsoft.com/zh-cn/azure/ai-services/openai/reference#chat-completions
[azure-openai-url]: https://learn.microsoft.com/zh-cn/azure/ai-services/openai/concepts/models
[rfc]: https://github.com/lobehub/lobe-chat/discussions/178
