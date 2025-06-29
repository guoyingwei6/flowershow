---
created: 2024-08-02 11:16
updated: 2025-06-10 09:54
dg-publish: true
---
例如，如果您通过[st.header()](https://docs.streamlit.io/develop/api-reference/text/st.header)命令定义标题文本，如下所示：
```python
st.header("Section 1")
```



然后您可以使用以下命令创建指向此标头的链接：
```python
st.markdown("[Section 1](#section-1)")
```

