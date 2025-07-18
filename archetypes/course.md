---
docType: "Course"
title: "1. {{ replace .File.ContentBaseName `-` ` ` | title }}" # Note: You need to manually modify the number prefix
description: "Enter a detailed introduction to the course here..."
lectures: 0 # Please manually update the number of lectures
courseTitle: '{{ replace .File.ContentBaseName `-` ` ` | title }}'
themeColor: "#00B39F" # Default color used in the example
order: 1
cardImage: ""
toc:
  [
    # List the chapter filenames included in this course in order (without .md)
    # For example:
    # "experience-we-want-to-change",
    # "new-ideas-and-concepts",
  ]
---