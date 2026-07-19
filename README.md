# 🚀 Create & Publish Blog Post on WordPress using n8n

![n8n](https://img.shields.io/badge/n8n-Automation-FF6D5A?style=for-the-badge)
![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4o--mini-412991?style=for-the-badge)
![WordPress](https://img.shields.io/badge/WordPress-API-21759B?style=for-the-badge)
![SEO](https://img.shields.io/badge/SEO-Optimized-0F9D58?style=for-the-badge)
![AI](https://img.shields.io/badge/AI-Blog%20Generation-success?style=for-the-badge)
![Publishing](https://img.shields.io/badge/Publishing-Automated-blue?style=for-the-badge)
![Google Sheets](https://img.shields.io/badge/Google-Sheets-34A853?style=for-the-badge)

---

# 📖 Overview

This n8n workflow automatically generates and publishes blog posts on a WordPress website using AI.

The workflow fetches the latest technology news, generates a unique blog article using OpenAI, creates a featured image, uploads the image to WordPress, and publishes the article automatically.

This automation eliminates manual content creation and publishing efforts while ensuring a consistent flow of fresh content.

---

# 🖼️ Workflow Layout

![Workflow Architecture](images/blog-post-wordpress.png)

---

# ✨ Features

- 📅 Automatic scheduled execution
- 📰 Fetch latest technology news
- 🤖 AI-powered blog generation
- 🎨 Automatic featured image generation
- 🖼️ Image resizing for blog compatibility
- 📤 Upload image to WordPress Media Library
- 🚀 Publish post automatically
- 🏷️ Assign featured image automatically
- 🔄 Fully automated content pipeline

---

# 🌍 Use Cases

### 📈 Content Marketing

Automatically publish trending technology articles.

### 🔍 SEO Blogging

Generate regular blog content to improve website visibility.

### 📰 News Websites

Repurpose trending news into unique blog articles.

### 🏢 Agency Automation

Manage multiple client blogs with minimal manual effort.

### ✍️ Personal Blogs

Maintain a consistently active blog.

---

# ⚙️ Workflow Nodes

## 1️⃣ ⏰ Schedule Trigger

**Node Type:** Schedule Trigger

### 🎯 Purpose

Starts the workflow automatically at a configured time.

### ⚙️ Configuration

- 📅 Runs once daily
- 🕘 Configured execution hour: **9 AM**

### 📤 Expected Output

Triggers workflow execution.

---

## 2️⃣ 🌐 HTTP Request (Fetch News)

**Node Type:** HTTP Request

### 🎯 Purpose

Retrieves the latest technology news articles.

### 📡 Method

`GET`

### 🔗 API Endpoint

```text
https://newsdata.io/api/1/news
```

### 📋 Query Parameters

| Parameter | Value |
|------------|---------|
| apikey | YOUR_NEWSDATA_API_KEY |
| category | technology |
| language | en |

### 📤 Output

Returns the latest technology news articles.

### 📄 Example

```json
{
  "title": "AI Startup Raises Funding",
  "description": "A new AI startup has secured..."
}
```

---

## 3️⃣ 🤖 OpenAI (Blog Generator)

**Node Type:** OpenAI Chat Model

### 🎯 Purpose

Converts news content into an original blog article.

### 🧠 Model

`GPT-4o`

### 📥 Input

- News title
- News description

### 📝 Prompt Responsibilities

- ✨ Generate unique title
- 📄 Create 5 HTML paragraphs
- 🚫 Avoid plagiarism
- 📊 Add analysis
- 🎯 Create conclusion
- 📦 Return valid JSON

### 📤 Output Example

```json
{
  "title": "How AI Startups Are Reshaping Innovation",
  "content": "<p>...</p>"
}
```

---

## 4️⃣ 💻 Code Node

**Node Type:** Code

### 🎯 Purpose

Parses JSON returned by OpenAI.

### ⚡ Operations

- 📝 Extract title
- 📄 Extract content
- 🧩 Create clean JSON structure

### 📤 Output

```json
{
  "title": "...",
  "content": "..."
}
```

---

## 5️⃣ 🎨 OpenAI Image Generation

**Node Type:** OpenAI Image

### 🎯 Purpose

Generate a blog featured image.

### 📝 Prompt Source

- Generated title
- First portion of blog content

### 💡 Example Prompt

> Create a professional illustration showing advancements in artificial intelligence and modern technology.

### 🖼️ Output

Generated image file.

---

## 6️⃣ 🖼️ Edit Image

**Node Type:** Edit Image

### 🎯 Purpose

Resize image before uploading.

### ⚙️ Configuration

- 📐 Width: **1340 px**
- 📏 Height: **638 px**

### ✅ Benefits

- 🎨 Consistent blog appearance
- 🌐 Optimized WordPress display
- 📱 Social sharing compatibility

### 📤 Output

Resized image.

---

## 7️⃣ 📤 HTTP Request (Upload Media)

**Node Type:** HTTP Request

### 🎯 Purpose

Uploads the image to the WordPress Media Library.

### 📡 Method

`POST`

### 🔗 Endpoint

```text
https://your-wordpress-site.com/wp-json/wp/v2/media
```

### 🔐 Authentication

WordPress API

### 📑 Headers

```text
Content-Disposition:
attachment; filename=image.png
```

### 📤 Output

```json
{
  "id": 123
}
```

The returned **Media ID** is later used as the featured image.

---

## 8️⃣ 🔀 Merge

**Node Type:** Merge

### 🎯 Purpose

Combines:

- 📄 Blog content
- 🖼️ Uploaded image ID

### 📤 Result

Creates a single payload ready for publishing.

---

## 9️⃣ 🚀 HTTP Request (Publish Post)

**Node Type:** HTTP Request

### 🎯 Purpose

Publishes the blog post to WordPress.

### 📡 Method

`POST`

### 🔗 Endpoint

```text
https://your-wordpress-site.com/wp-json/wp/v2/posts
```

### 📦 Request Body

```json
{
  "title": "...",
  "content": "...",
  "status": "publish",
  "featured_media": 123
}
```

### 📤 Output

Published WordPress article.

---

# 🔐 Required Credentials

## 🤖 OpenAI

### Required

- OpenAI API Key

### Used For

- Blog generation
- Image generation

---

## 📰 NewsData API

### Required

- NewsData API Key

### Used For

- Fetching latest news

---

## 🌐 WordPress

### Required

- Website URL
- Username
- Application Password

### Used For

- Media uploads
- Blog publishing

---

# 🚀 Installation

### 1️⃣ Import workflow.json into n8n

### 2️⃣ Create OpenAI credential

### 3️⃣ Add your OpenAI API key

### 4️⃣ Create NewsData API key

### 5️⃣ Update the HTTP Request node

Replace:

```text
YOUR_NEWSDATA_API_KEY
```

with your actual key.

### 6️⃣ Create WordPress credential

### 7️⃣ Generate an Application Password in WordPress

### 8️⃣ Update WordPress URLs

Replace:

```text
https://your-wordpress-site.com
```

with your website URL.

### 9️⃣ Run the workflow manually

### 🔟 Enable the workflow

---

# 🛠️ Customization

## 📰 Change News Category

Examples:

- business
- sports
- health
- entertainment
- science

---

## ⏰ Change Posting Frequency

Modify the Schedule Trigger.

Examples:

- Every hour
- Daily
- Weekly

---

## 📄 Improve Blog Length

Update the OpenAI prompt.

Example:

> Generate a 1000-word article.

---

## 🔍 Add SEO Keywords

Inject keywords into the OpenAI prompt.

---

## 📝 Publish Draft Instead

Change:

```text
publish
```

to:

```text
draft
```

---

# ⚠️ Troubleshooting

## ❌ OpenAI Error

**Cause:** Invalid API key.

**Solution:** Verify your OpenAI credentials.

---

## ❌ WordPress Authentication Error

**Cause:** Invalid Application Password.

**Solution:** Generate a new Application Password.

---

## ❌ No News Returned

**Cause:** API quota exceeded.

**Solution:** Upgrade your NewsData plan or retry later.

---

## ❌ Image Upload Failure

**Cause:** WordPress upload restrictions.

**Solution:** Increase the upload size limit on your WordPress server.

---

# 🛠️ Technologies Used

- 🤖 n8n
- 🧠 OpenAI GPT-4o
- 🎨 OpenAI Image Generation
- 📰 NewsData API
- 🌐 WordPress REST API
- 💻 JavaScript

---

# 🚀 Future Improvements

- 🔍 SEO metadata generation
- 🏷️ Auto category assignment
- 🔖 Auto tag assignment
- 📱 Social media posting
- 🌍 Multi-language support
- 🔗 Internal linking generation
- 📧 Newsletter integration

---

# ⭐ Support

If you found this project useful, consider giving it a **⭐ Star** on GitHub!

---

# 📜 License

This project is licensed under the **MIT License**.
