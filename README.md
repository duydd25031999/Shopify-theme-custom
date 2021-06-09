# Theme Kit
Cross-platform command line tool to build Shopify themes

## Setup Theme Kit

1. ### Install Theme Kit
    `Window`
    1. Install  `Chocolatey`
    1. Install `Theme Kit` with `Chocolatey`
        ```command
        choco install themekit
        ```
        - If `Chocolatey` install = quyền admin =>  install `Theme Kit` cũng phải dùng quyền admin
1. ### Generate API credentials
    1. Click `App`

        ![Click App](./doc_assets/Capture1.PNG)

    1. Click `Manage private apps`

        ![Click Manage private apps](./doc_assets/Capture2.PNG)
    
    1. If private app development is disabled, then click Enable private app development. Only the store owner can enable private app development.
        - Chọn cả 3 options mới được enable.

    1. Click Create new private app.

    1. In the App details section, fill out the app name and your email address.

    1. In the `Admin API section`, click `Show inactive Admin API permissions`.

    1. Scroll to the `Themes` section and select `Read and write` from the dropdown.

        ![Scroll to the Themes section](./doc_assets/Capture3.PNG)

    1. Click `Save`.

    1. Read the private app confirmation dialog, then click `Create app`.

1. ### Connect to an existing theme
    ```command
    theme get --password=[your-password] --store=[your-store.myshopify.com] --themeid=[your-theme-id]
    ```

    `password`
    ![your-password](./doc_assets/Capture4.PNG)

    `themeid`    
    1. Go to `Online Store/Themes`

        ![Go to Online Store/Themes](./doc_assets/Capture5.PNG)

    1. Click `Customize`

        ![Click Customize](./doc_assets/Capture6.PNG)

    1. Copy Theme ID on URL

        ![Copy Theme ID on URL](./doc_assets/Capture7.PNG)

1. ### Watch dev change

```command
theme watch --allow-live
```

## Create new theme

cmd
```command
theme new --password=[your-password] --store=[your-store.myshopify.com] --name=[your-theme-name]
theme new --password=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 --store=duy-michael.myshopify.com --name="duy-michael-1"
```

# Liquid
Liquid is an open-source template language created by Shopify and written in Ruby.

[Liquid document](https://www.shopify.com/partners/shopify-cheat-sheet)

## Folder structure
Shopify chỉ hiểu những folder bên dưới
- Nếu tạo mới 1 folder không có dưới đây, Shopify sẽ không đọc
- Nếu tạo subfolder, Shopify cũng không đọc

### assets
Chứa js, css, image, font, ...

`.scss.liquid`
- Là file liquid dùng để xử lý scss
- Code y như scss bình thường
- Thay thế cho scss-loader

### config
Chứa setting cho theme

`settings_schema.json`
- Code cho phần Theme settings

    ![Theme settings](./doc_assets/Capture8.PNG)

    ```json
    [ // list các dropdowns
        {...}, // item 0 là metadata cho settings_schema.json => không tính là dropdowns
        {
            'name': [...] //List title của dropdown theo ngôn ngữ
            'settings': [] //content của dropdown
        }
    ]
    ```

- Global settings
    - Lấy giá trị trong code = settings.id_của_settings
        
- `type` :
    - Cách để lựa chọn value
    - `header` : Tên của 1 sub list trong 1 dropdown
        - header sẽ nhóm các items khác giữa nó với Header tiếp theo vào 1 nhóm

        ![type: header](./doc_assets/Capture9.PNG)

    ```json
    "settings": [
        {
            "type": "header",
            "content": {
                "en": "Text",
            }
        },
        {
            "type": "color",
            "id": "color_text", 
            "label": {
                "en": "Headings and links",
            },
            "default": "#3a3a3a"
        },
        {
            "type": "color",
            "id": "color_body_text",
            "label": {
                "en": "Body text"
            },
            "default": "#333232"
        },
        {
            "type": "color",
            "id": "color_sale_text",
            "label": {
                "en": "Sale price"
            },
            "default": "#EA0606"
        },
        {
            "type": "header",
            "content": {
                "en": "Buttons"
            }
        },
        ...
    ]
    ```

### layout
Master page
- Những subpages sẽ inherit from that layout

`theme.liquid`
- Master layout mà mọi page sẽ phải inherit tới

### locales
Chứa message theo từng thứ tiếng

### sections
Chứa các sections
- Trong page có schema để setting section
    ```js
    section.settings.id_của_setting // lấy value của setting riêng của section

    {% schema %}
    {
        name: [...], // tên của section theo từng lang
        // tên sẽ được hiện cứng ở danh sách sections của page
        presets: [ // Hiển thị lúc chọn "Add Section"
            {
                category: "Custom Content", // Tên nhóm section
                name: "Text", // Tên của dsection lúc lựa chọn
            }
        ],
        settings: [ // setting cho section đó
            {

            },
            ...
        ]
    }
    {% endschema %}
    ```

Tạo 1 file mới trong s

Khi tạo mới một section trong page
- Lựa chọn 1 trong những section có sẵn

Trong mỗi section có nhiều components con
- Mỗi component sẽ được config khác nhau

### snippets
Component có thể dùng lại nhiều lần

### templates
Mỗi file tương đương với 1 page



Default template file
- 404.liquid
- article.liquid
- blog.liquid
- cart.liquid
- checkout.liquid
- collection.liquid
    - Template cho collection
    - Tạo 1 template collection khác
        - Tạo 1 file `collection.[tên custom].liquid`
        - Khi tạo collection trên admin => dropdown Theme templates
- customers
    - account.liquid
    - activate_account.liquid
    - addresses.liquid
    - login.liquid
    - order.liquid
    - register.liquid
    - reset_password.liquid
- gift_card.liquid
- index.liquid
- list-collections.liquid
- page.liquid
- password.liquid
- product.liquid
    - Template cho product
    - Tạo 1 template product khác
        - Tạo 1 file `product.[tên custom].liquid`
        - Khi tạo product trên admin => dropdown Theme templates
- search.liquid
- theme.liquid

## Global variable

`shop`
- Tương đương với store

`settings`
- Lấy giá trị từ `Theme Setting`