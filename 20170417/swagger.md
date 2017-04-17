### swagger 规范
- 能看懂swagger 规范文档
- eg:

```
swagger: '2.0'  
info:  
  title: Uber API  
  description: Move your app forward with the Uber API  
  version: "1.0.0"  

／* the domain of the service  接口文档的域名*／  
host: api.uber.com  

/* array of all schemes that your API supports 下面所有的接口都支持的协议*/
schemes:
  - https
  - http

／* will be prefixed to all paths 需要填充的路径*／
basePath: /v1  ／* bathPath with something 基础的路径*／
produces:  ／* will fallback type 返回的数据类型*／
  - application/json  
paths:  
  /products:  ／* 接口地址是： https:// + host + basePath + ／products *／  
    get:  ／* 接口类型*／
      summary: Product Types  ／* 简介描述接口 *／
      description: | ／* 接口的详细描述 *／
        The Products endpoint returns information about the *Uber* products
        offered at a given location. The response includes the display name
        and other details about each product, and lists the products in the
        proper display order.
      parameters: ／* 接口所需传入参数*／
        - name: latitude
          in: query
          description: Latitude component of location.
          required: true
          type: number
          format: double
        - name: longitude
          in: query
          description: Longitude component of location.
          required: true
          type: number
          format: double
      tags:
        - Products ／* 接口所需作用 *／
      responses: ／* 接口返回 *／
        200:
          description: An array of products
          schema:
            type: array
            items:
              $ref: '#/definitions/Product' ／* 对应的数据结构 *／
        default:
          description: Unexpected error
          schema:
            $ref: '#/definitions/Error'
definitions: ／* 定义数据结构*／
  Product:
    type: object
    properties:
      product_id:
        type: string
        description: Unique identifier representing a specific product for a given latitude & longitude. For example, uberX in San Francisco will have a different product_id than uberX in Los Angeles.
      description:
        type: string
        description: Description of product.
      display_name:
        type: string
        description: Display name of product.
      capacity:
        type: string
        description: Capacity of product. For example, 4 people.
      image:
        type: string
        description: Image URL representing the product.
  Error:
    type: object
    properties:
      code:
        type: integer
        format: int32
      message:
        type: string
      fields:
        type: string

```
