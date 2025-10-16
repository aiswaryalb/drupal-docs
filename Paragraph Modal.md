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

## ✏️ Editing Paragraphs
When editing a paragraph, select the appropriate category:

- Open the paragraph you wish to edit.
- In the edit interface, select the category that fits the content of the paragraph.
 
## 📊Configure Content Type Display
To display paragraphs properly in the content type, follow these steps:

- Navigate to the content type that references the paragraph.
- Go to Manage form display.
- Locate the field referencing the paragraph and click on Edit.
- In the **Add Mode** section, select **Modal Form**.
- Under **Display Paragraphs in dialog**, choose **Tiles**.
- Save your changes.
