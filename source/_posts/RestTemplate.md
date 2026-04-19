---
title: RestTemplate
date: 2019-01-28
tags:
  - Spring
  - Java
toc: true
---

最近做一个关于设计个人博客的项目实训，其中需要实现上传照片功能，思考再三，觉得保存在本地着实很 low，于是选择上传到第三方图床。接下来的问题很明确，如何调用第三方接口，首先想到的是大名鼎鼎的 HTTPClient，但 Spring 封装了库，提供了更为简洁的资源请求方式 RestTemplate。

## API 接口

查看第三方图床 API 文档，上传图片为 POST 方法，参数暂时只需 File。
![upload-api](upload-api.webp)

![upload-successfully](upload-successfully.webp)

### 请求 API

```java
private String uploadViaSmms(MultipartFile file) {

    // 支持中文
    restTemplate.getMessageConverters().set(1, new StringHttpMessageConverter(StandardCharsets.UTF_8));

    String requestUrl = "https://sm.ms/api/upload";

    MultiValueMap<String, Object> postParameters = new LinkedMultiValueMap<>();
    postParameters.add("smfile", file.getResource());

    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(MediaType.MULTIPART_FORM_DATA);
    headers.setAccept(Arrays.asList(MediaType.APPLICATION_JSON));
    headers.add("user-agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36");

    HttpEntity<MultiValueMap<String, Object>> requestEntity = new HttpEntity<>(postParameters, headers);
    ResponseEntity<String> response = restTemplate.postForEntity(requestUrl, requestEntity, String.class);

    return response.getBody();
}
```

### 处理 JSON 数据

```java
Result r = new Result();

String result = uploadViaSmms(file);

JsonObject jsonObject = new JsonParser().parse(result).getAsJsonObject();
String code = jsonObject.get("code").getAsString();
if(!code.equals("success")) {
    r.setResultCode(ResultCode.UPLOAD_ERROR);
    return r;
}
String url = jsonObject.get("data").getAsJsonObject().get("url").getAsString();
String deleteUrl = jsonObject.get("data").getAsJsonObject().get("delete").getAsString();

imageService.upload(url, deleteUrl, description);
r.setResultCode(ResultCode.SUCCESS);

return r;
```

## 需要注意的问题

- 如果 headers 不添加 `user-agent`，则会报 **403 Forbidden**，也算一种常见的反爬措施。
  ![403-forbidden](403-forbidden.webp)
- file 类型必须是可序列化元素，只能使用 `file.getResource()`，不能使用 `file.getBytes()` 或 `file.getInputStream()`，否则会报 **No serializer found**。
  ![no-serializer-found](no-serializer-found.webp)
  ~~↑这个问题困扰了我两天，晚上 4 点都睡不着都是因为这，コノヤロー！~~

## 参考资料

1. [403 Forbidden](https://stackoverflow.com/questions/44922261/why-do-i-always-get-403-when-fetching-data-with-resttemplate)
2. [parse JSON](https://stackoverflow.com/questions/2591098/how-to-parse-json-in-java)
