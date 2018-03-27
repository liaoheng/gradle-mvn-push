javadoc编码GBK的字符无法映射
===============

在gradle.build文件中加入：
javadoc {
    options {
        encoding "UTF-8"
        charSet 'UTF-8'
        author true
    }
}