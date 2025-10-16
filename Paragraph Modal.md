# 📜 Paragraph Modal and Grouping in Drupal

## 📖 Overview
This document outlines the steps to implement the Paragraphs Editor Enhancements module in Drupal, allowing for better categorization and dialog display for paragraphs.

## ⚙️ Module Installation
To begin, install the **Paragraphs Editor Enhancements** module using Composer. Run the following command in your terminal:

```bash
composer require 'drupal/paragraphs_ee:^10.0'
```
## 🗂️ Creating Paragraph Categories
After successfully installing the module, create your paragraph categories:
- Navigate to **/admin/structure/paragraphs_category**.
- Add the desired categories needed for your paragraphs.
<img width="2599" height="867" alt="image" src="https://github.com/user-attachments/assets/310d88d2-4547-444d-b750-e93a44879523" />

## ✏️ Editing Paragraphs
When editing a paragraph, select the appropriate category:

- Open the paragraph you wish to edit.
- In the edit interface, select the category that fits the content of the paragraph.
<img width="1947" height="1207" alt="image" src="https://github.com/user-attachments/assets/8731bb64-e96a-4b0a-b532-17235babe265" />

## 📊Configure Content Type Display
To display paragraphs properly in the content type, follow these steps:
<img width="2005" height="3196" alt="image" src="https://github.com/user-attachments/assets/cdbb18da-6d27-48f2-8d2f-dc57fbba6774" />

- Navigate to the content type that references the paragraph.
- Go to Manage form display.
- Locate the field referencing the paragraph and click on Edit.
- In the **Add Mode** section, select **Modal Form**.
- Under **Display Paragraphs in dialog**, choose **Tiles**.
- Save your changes.
