<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>CardápioJá</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>CardápioJá</h1>
  </header>

  <main id="menu">
    <!-- Produtos serão carregados aqui -->
  </main>

  <footer>
    <a href="admin.html">Área Administrativa</a>
  </footer>

  <script src="script.js"></script>
</body>
</html>

<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Administração - CardápioJá</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>Painel Administrativo</h1>
  </header>

  <main>
    <h2>Adicionar Produto</h2>
    <form id="form-add">
      <input type="text" id="name" placeholder="Nome do prato" required>
      <input type="text" id="price" placeholder="Preço" required>
      <button type="submit">Adicionar</button>
    </form>

    <h2>Produtos Cadastrados</h2>
    <ul id="product-list"></ul>
  </main>

  <footer>
    <a href="index.html">Voltar ao Cardápio</a>
  </footer>

  <script src="script.js"></script>
</body>
</html>

body {
  font-family: Arial, sans-serif;
  margin: 0;
  background-color: #f0fdf4;
  color: #064e3b;
}

header, footer {
  background-color: #10b981;
  color: white;
  text-align: center;
  padding: 1rem;
}

main {
  padding: 1rem;
}

form {
  margin-bottom: 2rem;
}

input {
  padding: 0.5rem;
  margin: 0.5rem 0;
  display: block;
  width: 100%;
  max-width: 300px;
}

button {
  padding: 0.7rem 1.5rem;
  background-color: #059669;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #047857;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  background: #d1fae5;
  margin: 0.5rem 0;
  padding: 0.7rem;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

a {
  color: white;
  text-decoration: none;
}

let products = JSON.parse(localStorage.getItem('products')) || [];

function saveProducts() {
  localStorage.setItem('products', JSON.stringify(products));
}

function renderProducts() {
  const menu = document.getElementById('menu');
  const productList = document.getElementById('product-list');

  if (menu) {
    menu.innerHTML = '';
    products.forEach(product => {
      const item = document.createElement('div');
      item.innerHTML = `<h3>${product.name}</h3><p>R$ ${product.price}</p>`;
      menu.appendChild(item);
    });
  }

  if (productList) {
    productList.innerHTML = '';
    products.forEach((product, index) => {
      const li = document.createElement('li');
      li.innerHTML = `
        ${product.name} - R$ ${product.price}
        <button onclick="editProduct(${index})">Editar</button>
        <button onclick="deleteProduct(${index})">Excluir</button>
      `;
      productList.appendChild(li);
    });
  }
}

function addProduct(name, price) {
  products.push({ name, price });
  saveProducts();
  renderProducts();
}

function deleteProduct(index) {
  products.splice(index, 1);
  saveProducts();
  renderProducts();
}

function editProduct(index) {
  const newName = prompt("Novo nome do prato:", products[index].name);
  const newPrice = prompt("Novo preço:", products[index].price);
  if (newName && newPrice) {
    products[index].name = newName;
    products[index].price = newPrice;
    saveProducts();
    renderProducts();
  }
}

document.addEventListener('DOMContentLoaded', () => {
  renderProducts();

  const form = document.getElementById('form-add');
  if (form) {
    form.addEventListener('submit', (e) => {
      e.preventDefault();
      const name = document.getElementById('name').value;
      const price = document.getElementById('price').value;
      addProduct(name, price);
      form.reset();
    });
  }
});
