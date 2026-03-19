根源分析
由于手动编辑或工具冲突，
assets/css/custom.scss
 的 Front Matter（开头的 ---）被合并到了同一行，且 Sass 变量的赋值格式被破坏（包含了多行大括号），导致 Jekyll 无法识别这是需要编译的 SCSS 文件，或者编译时抛出语法错误。

修复动作
重置文件结构：严格确保文件开头的前两行是标准的、独立成行的 ---。
修复 Liquid 变量：将所有的 site 变量引用重写为正确的单行格式，例如：$body-color: {{ site.body_background_color }};。
重新同步动画速度：保留了您之前要求的背景光晕 2倍渲染速度 的调整。
强制编译验证：执行了 bundle exec jekyll build，确认 
_site/assets/css/custom.css
 已成功生成且内容正确。
重要提示
请避免再次手动将开头的 --- 改为同一行（如 --- ---），并确保 Sass 变量后面紧跟正确的 Liquid 标签 {{ ... }}。

现在您运行 bundle exec jekyll serve 后，页面样式应该能正常加载且光晕动效会更快。