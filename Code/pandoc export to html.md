---
tags:
  - "#pandoc"
---
<span style="font-size:12px; color:#888888;">Created: 23.10.2024 19:47</span>

[[pandoc]]
```shell
pandoc '.\docs\admin_manual.md' -f markdown -t html -s -o '.\admin_manual.html' --embed-resources --standalone --css="docs\github-pandoc.css" --resource-path=docs
```


 - embed-resources - встроит все ресурсы в конечный файл  
 - standalone - будет итоговый один файл
 - css="docs\github-pandoc.css" - кастомный css
 - resource-path=docs - путь до каталога с ресурсами