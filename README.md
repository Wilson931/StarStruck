# StarStruck
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StarStruck - Personal Brand Clothing</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            color: #333;
        }

        nav {
            background-color: #111;
            color: white;
            padding: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        nav ul {
            list-style-type: none;
            margin: 0;
            padding: 0;
            display: flex;
        }

        nav ul li {
            margin-right: 20px;
        }

        nav ul li a {
            color: white;
            text-decoration: none;
        }

        section {
            padding: 50px;
        }

        .product-list {
            display: flex;
            flex-wrap: wrap;
        }

        .product {
            background-color: white;
            border: 1px solid #ddd;
            border-radius: 5px;
            padding: 20px;
            margin: 10px;
            width: 200px;
        }

        .language-switch {
            position: absolute;
            top: 20px;
            right: 20px;
        }

        form {
            margin-top: 20px;
        }

        input,
        button {
            margin: 5px;
            padding: 5px;
        }
    </style>
</head>

<body>
    <!-- 语言切换按钮 -->
    <div class="language-switch">
        <button id="en">English</button>
        <button id="cn">中文</button>
    </div>
    <!-- 导航栏 -->
    <nav>
        <h1>StarStruck</h1>
        <ul>
            <li><a href="#">Home</a></li>
            <li><a href="#spring">Spring</a></li>
            <li><a href="#summer">Summer</a></li>
            <li><a href="#autumn">Autumn</a></li>
            <li><a href="#winter">Winter</a></li>
        </ul>
    </nav>
    <!-- 季节分类展示 -->
    <section id="spring">
        <h2>Spring Collection</h2>
        <div class="product-list" id="spring-products"></div>
    </section>
    <section id="summer">
        <h2>Summer Collection</h2>
        <div class="product-list" id="summer-products"></div>
    </section>
    <section id="autumn">
        <h2>Autumn Collection</h2>
        <div class="product-list" id="autumn-products"></div>
    </section>
    <section id="winter">
        <h2>Winter Collection</h2>
        <div class="product-list" id="winter-products"></div>
    </section>
    <!-- 管理表单 -->
    <section id="admin">
        <h2>Admin Panel</h2>
        <form id="add-product-form">
            <input type="text" id="add-name" placeholder="Product Name">
            <input type="number" id="add-price" placeholder="Price">
            <input type="text" id="add-image" placeholder="Image URL">
            <select id="add-season">
                <option value="spring">Spring</option>
                <option value="summer">Summer</option>
                <option value="autumn">Autumn</option>
                <option value="winter">Winter</option>
            </select>
            <button type="submit">Add Product</button>
        </form>
        <form id="update-product-form">
            <input type="number" id="update-id" placeholder="Product ID">
            <input type="text" id="update-name" placeholder="New Product Name">
            <input type="number" id="update-price" placeholder="New Price">
            <input type="text" id="update-image" placeholder="New Image URL">
            <select id="update-season">
                <option value="spring">Spring</option>
                <option value="summer">Summer</option>
                <option value="autumn">Autumn</option>
                <option value="winter">Winter</option>
            </select>
            <button type="submit">Update Product</button>
        </form>
    </section>

    <script>
        const seasons = ['spring', 'summer', 'autumn', 'winter'];

        // 渲染商品列表
        async function renderProducts(season) {
            const productList = document.getElementById(`${season}-products`);
            productList.innerHTML = "";
            try {
                const response = await fetch(`http://localhost:3001/products/${season}`);
                const products = await response.json();
                products.forEach(product => {
                    const productDiv = document.createElement("div");
                    productDiv.classList.add("product");
                    productDiv.innerHTML = `
                        <img src="${product.image}" alt="${product.name}" width="100%">
                        <h3>${product.name}</h3>
                        <p>$${product.price}</p>
                    `;
                    productList.appendChild(productDiv);
                });
            } catch (error) {
                console.error('Error fetching products:', error);
            }
        }

        // 初始化页面
        async function init() {
            for (const season of seasons) {
                await renderProducts(season);
            }
        }

        init();

        // 添加商品
        const addProductForm = document.getElementById('add-product-form');
        addProductForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const name = document.getElementById('add-name').value;
            const price = parseFloat(document.getElementById('add-price').value);
            const image = document.getElementById('add-image').value;
            const season = document.getElementById('add-season').value;
            try {
                const response = await fetch('http://localhost:3001/products', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ name, price, image, season })
                });
                const result = await response.json();
                console.log(result);
                renderProducts(season);
            } catch (error) {
                console.error('Error adding product:', error);
            }
        });

        // 更新商品
        const updateProductForm = document.getElementById('update-product-form');
        updateProductForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            const id = parseInt(document.getElementById('update-id').value);
            const name = document.getElementById('update-name').value;
            const price = parseFloat(document.getElementById('update-price').value);
            const image = document.getElementById('update-image').value;
            const season = document.getElementById('update-season').value;
            try {
                const response = await fetch(`http://localhost:3001/products/${id}`, {
                    method: 'PUT',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ name, price, image, season })
                });
                const result = await response.json();
                console.log(result);
                renderProducts(season);
            } catch (error) {
                console.error('Error updating product:', error);
            }
        });

        // 语言切换功能
        const enButton = document.getElementById('en');
        const cnButton = document.getElementById('cn');

        const englishTexts = {
            spring: 'Spring Collection',
            summer: 'Summer Collection',
            autumn: 'Autumn Collection',
            winter: 'Winter Collection',
            admin: 'Admin Panel',
            addName: 'Product Name',
            addPrice: 'Price',
            addImage: 'Image URL',
            addSeason: 'Season',
            addButton: 'Add Product',
            updateId: 'Product ID',
            updateName: 'New Product Name',
            updatePrice: 'New Price',
            updateImage: 'New Image URL',
            updateSeason: 'Season',
            updateButton: 'Update Product'
        };

        const chineseTexts = {
            spring: '春季系列',
            summer: '夏季系列',
            autumn: '秋季系列',
            winter: '冬季系列',
            admin: '管理面板',
            addName: '商品名称',
            addPrice: '价格',
            addImage: '图片链接',
            addSeason: '季节',
            addButton: '添加商品',
            updateId: '商品 ID',
            updateName: '新商品名称',
            updatePrice: '新价格',
            updateImage: '新图片链接',
            updateSeason: '季节',
            updateButton: '更新商品'
        };

        function switchLanguage(texts) {
            document.querySelector('#spring h2').textContent = texts.spring;
            document.querySelector('#summer h2').textContent = texts.summer;
            document.querySelector('#autumn h2').textContent = texts.autumn;
            document.querySelector('#winter h2').textContent = texts.winter;
            document.querySelector('#admin h2').textContent = texts.admin;
            document.getElementById('add-name').placeholder = texts.addName;
            document.getElementById('add-price').placeholder = texts.addPrice;
            document.getElementById('add-image').placeholder = texts.addImage;
            document.querySelector('#add-season option[value="spring"]').textContent = texts.spring;
            document.querySelector('#add-season option[value="summer"]').textContent = texts.summer;
            document.querySelector('#add-season option[value="autumn"]').textContent = texts.autumn;
            document.querySelector('#add-season option[value="winter"]').textContent = texts.winter;
            document.querySelector('#add-product-form button').textContent = texts.addButton;
            document.getElementById('update-id').placeholder = texts.updateId;
            document.getElementById('update-name').placeholder = texts.updateName;
            document.getElementById('update-price').placeholder = texts.updatePrice;
            document.getElementById('update-image').placeholder = texts.updateImage;
            document.querySelector('#update-season option[value="spring"]').textContent = texts.spring;
            document.querySelector('#update-season option[value="summer"]').textContent = texts.summer;
            document.querySelector('#update-season option[value="autumn"]').textContent = texts.autumn;
            document.querySelector('#update-season option[value="winter"]').textContent = texts.winter;
            document.querySelector('#update-product-form button').textContent = texts.updateButton;
        }

        enButton.addEventListener('click', () => {
            switchLanguage(englishTexts);
        });

        cnButton.addEventListener('click', () => {
            switchLanguage(chineseTexts);
        });
    </script>
</body>

</html>
