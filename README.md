<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StarStruck - 个人品牌服装</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- 导航栏 -->
    <nav>
        <div class="logo">StarStruck</div>
        <div class="language-switch">
            <button id="lang-en">English</button>
            <button id="lang-cn">中文</button>
        </div>
    </nav>

    <!-- 四季分类 -->
    <div class="season-container">
        <div class="season" id="spring">
            <h2>春季</h2>
            <img src="spring.jpg" alt="春季">
        </div>
        <div class="season" id="summer">
            <h2>夏季</h2>
            <img src="summer.jpg" alt="夏季">
        </div>
        <div class="season" id="autumn">
            <h2>秋季</h2>
            <img src="autumn.jpg" alt="秋季">
        </div>
        <div class="season" id="winter">
            <h2>冬季</h2>
            <img src="winter.jpg" alt="冬季">
        </div>
    </div>

    <script src="scripts.js"></script>
</body>
</html>
/* styles.css */
body {
    font-family: 'Arial', sans-serif;
    margin: 0;
    padding: 0;
    background-color: #181818;
    color: #fff;
}

nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px;
    background-color: #111;
}

.logo {
    font-size: 24px;
    font-weight: bold;
}

.language-switch button {
    background-color: #333;
    color: #fff;
    padding: 8px 15px;
    border: none;
    cursor: pointer;
}

.language-switch button:hover {
    background-color: #444;
}

.season-container {
    display: flex;
    justify-content: space-around;
    margin: 40px 0;
}

.season {
    width: 22%;
    text-align: center;
}

.season img {
    width: 100%;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
    transition: transform 0.3s ease;
}

.season:hover img {
    transform: scale(1.1);
}
// scripts.js
document.getElementById('lang-en').addEventListener('click', function() {
    document.documentElement.lang = 'en';
    // 你可以在这里更新页面内容为英文
});

document.getElementById('lang-cn').addEventListener('click', function() {
    document.documentElement.lang = 'zh';
    // 你可以在这里更新页面内容为中文
});
CREATE DATABASE starstruck;

USE starstruck;

CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    description TEXT,
    image_url VARCHAR(255) NOT NULL,
    season ENUM('spring', 'summer', 'autumn', 'winter') NOT NULL
);
<?php
// db.php - 数据库连接
$host = 'localhost';
$db = 'starstruck';
$user = 'root';
$pass = '';
$pdo = new PDO("mysql:host=$host;dbname=$db", $user, $pass);

// 添加商品
if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $name = $_POST['name'];
    $price = $_POST['price'];
    $description = $_POST['description'];
    $image_url = $_POST['image_url'];
    $season = $_POST['season'];

    $stmt = $pdo->prepare("INSERT INTO products (name, price, description, image_url, season) VALUES (?, ?, ?, ?, ?)");
    $stmt->execute([$name, $price, $description, $image_url, $season]);
    echo "商品添加成功!";
}

// 获取商品列表
$stmt = $pdo->query("SELECT * FROM products");
$products = $stmt->fetchAll();
?>

<!DOCTYPE html>
<html lang="zh">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StarStruck 后台管理</title>
</head>
<body>
    <h1>后台管理系统</h1>

    <form action="" method="POST">
        <label>商品名称：</label><input type="text" name="name" required><br>
        <label>价格：</label><input type="number" name="price" required><br>
        <label>描述：</label><textarea name="description" required></textarea><br>
        <label>图片URL：</label><input type="text" name="image_url" required><br>
        <label>季节：</label>
        <select name="season">
            <option value="spring">春季</option>
            <option value="summer">夏季</option>
            <option value="autumn">秋季</option>
            <option value="winter">冬季</option>
        </select><br>
        <button type="submit">添加商品</button>
    </form>

    <h2>商品列表</h2>
    <ul>
        <?php foreach ($products as $product) : ?>
            <li>
                <h3><?php echo $product['name']; ?></h3>
                <img src="<?php echo $product['image_url']; ?>" alt="<?php echo $product['name']; ?>" width="100">
                <p>价格：¥<?php echo $product['price']; ?></p>
                <p><?php echo $product['description']; ?></p>
            </li>
        <?php endforeach; ?>
    </ul>
</body>
</html>
