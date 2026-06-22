# StaticShop Catalog

A static JSON-based product catalog system designed for efficient product data management, inventory tracking, and catalog aggregation in e-commerce applications.

## Overview

StaticShop Catalog provides:
- Comprehensive product catalog in JSON format
- Multi-category product organization (home, electronics, sports, clothing, books)
- Real-time inventory tracking and stock management
- Product ratings and customer feedback
- Aggregated analytics by category
- Static file-based storage for simplicity
- GitHub Pages support for web hosting

## Features

- **Product Management**: 21+ products with detailed specifications
- **Multi-Category Support**: Home, Electronics, Sports, Clothing, Books
- **Inventory Tracking**: Real-time stock levels and inventory values
- **Product Ratings**: 5-star rating system for customer feedback
- **Category Analytics**: Aggregated inventory counts and values by category
- **Pricing Information**: Competitive pricing across all products
- **Static Hosting**: Deploy on GitHub Pages without backend infrastructure
- **JSON Structure**: Easy to integrate with any frontend framework

## Tech Stack

- **Data Format**: JSON (JavaScript Object Notation)
- **Hosting**: GitHub Pages (optional)
- **Version Control**: Git
- **Compatibility**: Language-agnostic (works with any programming language)

## Project Structure

```
staticshop-catalog/
├── catalog.json          # Product catalog database
├── README.md            # This file
├── .github/             # GitHub configuration
└── assets/              # Static assets (optional)
```

## Catalog Schema

### Metadata

```json
{
  "metadata": {
    "email": "24f3004981@ds.study.iitm.ac.in",
    "version": "e2d5804b"
  }
}
```

### Product Structure

```json
{
  "id": "prod-e2d5804b-001",
  "name": "Premium Item 1",
  "category": "home|electronics|sports|clothing|books",
  "price": 46.93,
  "stock": 150,
  "rating": 1.7
}
```

### Product Fields

| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique product identifier |
| name | string | Product name/title |
| category | string | Product category |
| price | number | Product price in USD |
| stock | number | Available quantity |
| rating | number | Customer rating (1-5 scale) |

### Category Aggregations

```json
{
  "aggregations": {
    "home": {
      "count": 4,
      "inventoryValue": 109485.67
    }
  }
}
```

## Current Inventory

### Overview

- **Total Products**: 21
- **Categories**: 5
- **Total Inventory Value**: $675,608.41
- **Average Product Rating**: 3.1/5

### Category Breakdown

| Category | Count | Inventory Value | Sample Products |
|----------|-------|-----------------|-----------------|
| Home | 4 | $109,485.67 | Premium Item 1, Elite Item 3 |
| Electronics | 7 | $287,344.60 | Pro Item 2, Premium Item 6 |
| Sports | 4 | $193,919.72 | Basic Item 5, Elite Item 20 |
| Clothing | 6 | $84,858.43 | Basic Item 7, Standard Item 17 |
| Books | 0 | $0.00 | — |

### Price Range

- **Lowest**: $37.47 (Standard Item 17)
- **Highest**: $491.26 (Basic Item 12)
- **Average**: $184.58

### Stock Analysis

- **Highest Stock**: 199 units (Standard Item 17)
- **Lowest Stock**: 3 units (Standard Item 13)
- **Average Stock**: 126 units
- **Out of Stock**: 0 products

### Top Rated Products

1. **Elite Item 15** - 4.6 ⭐ (Electronics, $79.60)
2. **Elite Item 3** - 4.5 ⭐ (Home, $184.16)
3. **Pro Item 4** - 4.5 ⭐ (Home, $422.84)
4. **Standard Item 13** - 4.4 ⭐ (Clothing, $185.06)
5. **Pro Item 2** - 4.4 ⭐ (Electronics, $256.26)

## Installation & Usage

### Direct Usage

1. Clone the repository:
```bash
git clone https://github.com/DS24F3004981/staticshop-catalog.git
cd staticshop-catalog
```

2. Open `catalog.json` to view the product data

3. Integrate with your application:

**JavaScript/Node.js**:
```javascript
const catalog = require('./catalog.json');
console.log(catalog.products);
```

**Python**:
```python
import json

with open('catalog.json', 'r') as f:
    catalog = json.load(f)
    print(catalog['products'])
```

**cURL**:
```bash
curl -s https://raw.githubusercontent.com/DS24F3004981/staticshop-catalog/main/catalog.json | jq '.products'
```

### API Integration

Use this catalog as a data source for your API:

**Express.js**:
```javascript
const express = require('express');
const catalog = require('./catalog.json');
const app = express();

app.get('/api/products', (req, res) => {
  res.json(catalog.products);
});

app.get('/api/products/:id', (req, res) => {
  const product = catalog.products.find(p => p.id === req.params.id);
  res.json(product);
});

app.listen(3000);
```

**Flask**:
```python
from flask import Flask, jsonify
import json

app = Flask(__name__)

with open('catalog.json') as f:
    catalog = json.load(f)

@app.route('/api/products')
def get_products():
    return jsonify(catalog['products'])

@app.route('/api/products/<product_id>')
def get_product(product_id):
    product = next((p for p in catalog['products'] if p['id'] == product_id), None)
    return jsonify(product)

app.run()
```

## Data Queries

### Filter by Category

**JavaScript**:
```javascript
const electronics = catalog.products.filter(p => p.category === 'electronics');
```

**Python**:
```python
electronics = [p for p in catalog['products'] if p['category'] == 'electronics']
```

### Sort by Price

```javascript
// Lowest to highest
const sorted = catalog.products.sort((a, b) => a.price - b.price);

// Highest to lowest
const sortedDesc = catalog.products.sort((a, b) => b.price - a.price);
```

### Find High-Rated Products

```javascript
const highRated = catalog.products.filter(p => p.rating >= 4.0);
```

### Check Inventory Status

```javascript
const lowStock = catalog.products.filter(p => p.stock < 50);
const outOfStock = catalog.products.filter(p => p.stock === 0);
```

### Calculate Category Totals

```javascript
const categoryTotals = {};
catalog.products.forEach(product => {
  if (!categoryTotals[product.category]) {
    categoryTotals[product.category] = { count: 0, value: 0 };
  }
  categoryTotals[product.category].count++;
  categoryTotals[product.category].value += product.price * product.stock;
});
```

## Updating the Catalog

### Adding a New Product

1. Open `catalog.json`
2. Add a new object to the `products` array:

```json
{
  "id": "prod-e2d5804b-022",
  "name": "New Product Name",
  "category": "home",
  "price": 199.99,
  "stock": 100,
  "rating": 4.2
}
```

3. Update aggregations if needed
4. Commit and push changes

### Updating Product Information

1. Find the product by ID
2. Update the relevant fields
3. Recalculate aggregations if inventory values changed
4. Commit changes with descriptive message

### Bulk Operations

**Update all prices by percentage**:
```python
import json

with open('catalog.json', 'r') as f:
    catalog = json.load(f)

# Apply 10% discount
for product in catalog['products']:
    product['price'] *= 0.90

with open('catalog.json', 'w') as f:
    json.dump(catalog, f, indent=2)
```

## Deployment

### GitHub Pages

The repository has GitHub Pages enabled for static hosting.

1. Push changes to `main` branch
2. Files are automatically published at: `https://DS24F3004981.github.io/staticshop-catalog/`

### Access catalog.json from GitHub Pages

```bash
curl https://DS24F3004981.github.io/staticshop-catalog/catalog.json
```

### CDN Integration

```html
<script>
  fetch('https://DS24F3004981.github.io/staticshop-catalog/catalog.json')
    .then(response => response.json())
    .then(catalog => console.log(catalog.products));
</script>
```

## Performance Optimization

### Caching

**HTTP Headers** for long-term caching:
```
Cache-Control: public, max-age=86400
```

### Compression

Gzip the JSON file for faster transmission:
```bash
gzip catalog.json
```

### File Size

- **Current Size**: ~3.1 KB
- **Gzipped Size**: ~1.2 KB

## Validation

### Schema Validation

Validate JSON structure:

```bash
# Using jq
jq empty catalog.json

# Using Python
python -m json.tool catalog.json > /dev/null
```

### Data Validation

Ensure data consistency:

```python
import json

with open('catalog.json') as f:
    catalog = json.load(f)

# Check all required fields
for product in catalog['products']:
    assert 'id' in product
    assert 'name' in product
    assert 'category' in product
    assert 'price' in product
    assert 'stock' in product
    assert 'rating' in product
    assert 0 <= product['rating'] <= 5
    assert product['stock'] >= 0
    assert product['price'] > 0

print("✓ All products valid")
```

## Statistics & Analytics

### Generate Reports

```python
import json
from collections import defaultdict

with open('catalog.json') as f:
    catalog = json.load(f)

# Category statistics
stats = defaultdict(lambda: {'count': 0, 'total_value': 0, 'avg_price': 0})

for product in catalog['products']:
    cat = product['category']
    stats[cat]['count'] += 1
    stats[cat]['total_value'] += product['price'] * product['stock']

for cat, data in stats.items():
    data['avg_price'] = data['total_value'] / data['count'] if data['count'] > 0 else 0
    print(f"{cat}: {data['count']} products, ${data['total_value']:.2f}")
```

## API Endpoints (Example)

If hosting with a backend API:

- `GET /api/v1/products` - Get all products
- `GET /api/v1/products/:id` - Get single product
- `GET /api/v1/products?category=electronics` - Filter by category
- `GET /api/v1/products?sort=price` - Sort products
- `GET /api/v1/categories` - Get category aggregations
- `GET /api/v1/products/search?q=query` - Search products

## Integration Examples

### E-Commerce Frontend

```javascript
async function loadCatalog() {
  const response = await fetch('./catalog.json');
  const catalog = await response.json();
  
  catalog.products.forEach(product => {
    renderProduct(product);
  });
}

function renderProduct(product) {
  const html = `
    <div class="product">
      <h3>${product.name}</h3>
      <p>Price: $${product.price}</p>
      <p>Stock: ${product.stock}</p>
      <p>Rating: ${product.rating} ⭐</p>
    </div>
  `;
  document.getElementById('products').innerHTML += html;
}
```

### Mobile App

```javascript
// React example
import { useState, useEffect } from 'react';

function ProductList() {
  const [products, setProducts] = useState([]);
  
  useEffect(() => {
    fetch('./catalog.json')
      .then(res => res.json())
      .then(catalog => setProducts(catalog.products));
  }, []);
  
  return (
    <div>
      {products.map(product => (
        <div key={product.id}>
          <h3>{product.name}</h3>
          <p>${product.price}</p>
        </div>
      ))}
    </div>
  );
}
```

## Best Practices

1. **Keep it Updated**: Update inventory regularly
2. **Version Control**: Use git to track changes
3. **Backup**: Maintain backups of catalog data
4. **Validation**: Validate data before commits
5. **Documentation**: Keep pricing and categories documented
6. **Aggregations**: Recalculate after bulk updates

## Contributing

1. Fork the repository
2. Create a feature branch
3. Update catalog.json
4. Commit with clear messages
5. Push and create a pull request

## License

This project is open source and available under the MIT License.

## Author

Created by DS24F3004981

## Resources

- [JSON Schema](https://json-schema.org/)
- [GitHub Pages Documentation](https://pages.github.com/)
- [REST API Design](https://restfulapi.net/)
