## 🌐 Universitas Cakrawala — Fullstack Project

Proyek fullstack yang terdiri dari backend Headless CMS menggunakan Strapi v5 dengan GraphQL, dan frontend menggunakan Astro yang melakukan SSG (Static Site Generation) dari data GraphQL.

### Struktur Repositori
- **cms-strapi**: Backend menggunakan Strapi v5 + GraphQL
- **frontend-universitas-cakrawala**: Frontend menggunakan Astro + GraphQL Fetch

Seluruh konten artikel, kategori, media, dan metadata dikelola melalui Strapi, lalu di-render statis oleh Astro.

### Backend — `cms-strapi`
Strapi CMS digunakan sebagai headless backend untuk mengelola artikel, kategori, serta media.

### Tech Stack
- **Strapi v5**
- **GraphQL Plugin**
- **SQLite** (default database)
- **Node.js ≥ 18**

### Instalasi
```bash
cd cms-strapi
npm install
```

### Menjalankan Strapi (Development)
```bash
npm run develop
```

### Akses
- **Panel Admin**: [http://localhost:1337/admin](http://localhost:1337/admin)
- **GraphQL Playground**: [http://localhost:1337/graphql](http://localhost:1337/graphql)

### Content Types

#### Article
| Field              | Type                    |
|--------------------|-------------------------|
| Title              | String                  |
| Slug               | UID                     |
| Content            | Rich Text JSON          |
| Author             | String                  |
| SEOMetaTitle       | String                  |
| SEOMetaDescription | String                  |
| CoverImage         | Media (Single)          |
| categories         | Relation (Many-to-Many) |

#### Category
| Field    | Type           |
|----------|----------------|
| Name     | String         |
| Slug     | UID            |
| articles | Many-to-Many   |

### Relasi
- **Article ↔ Categories**: Many-to-Many

### Pengaturan Permissions
Agar GraphQL bisa diakses publik:
1. Buka `Settings → Users & Permissions Plugin → Roles → Public`
2. Aktifkan izin:
   - **Article**: `find`, `findOne`
   - **Category**: `find`, `findOne`
3. Klik **Save**.

### Contoh Query GraphQL

Ambil semua artikel:
```graphql
query {
  articles {
    documentId
    Title
    Slug
  }
}
```

Ambil artikel berdasarkan slug:
```graphql
query ($slug: String!) {
  articles(filters: { Slug: { eq: $slug } }) {
    Title
    Slug
    Content
  }
}
```

Ambil kategori dan daftar artikelnya:
```graphql
query ($slug: String!) {
  categories(filters: { Slug: { eq: $slug } }) {
    Name
    articles {
      Title
      Slug
    }
  }
}
```

### Media Handling
- Strapi menghasilkan path relatif seperti `/uploads/image.jpg`
- Gunakan base URL di frontend: `http://localhost:1337/uploads/image.jpg`

### Build (Production)
```bash
npm run build
```
