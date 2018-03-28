javadoc编码GBK的字符无法映射
===============
出现这种问题的原因是javadoc生成文档时默认java文件的编码方式是GBK，而java源文件编码是UTF-8且文件中存在中文。解决方法是在生成javadoc时设置java文件的编码格式。对于Android和JAVA项目，解决方法略有不同：

## JAVA项目

在项目的gradle.build文件中加入：
```groovy
javadoc {
    options {
        encoding "UTF-8"
        charSet 'UTF-8'
        author true
    }
}
```

## Android项目

Android项目中，生成javadoc通常通过制定一个task androidJavadocs来完成，比如：
```groovy
task androidJavadocs(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
}
```

在这里加上文件编码格式声明即可：
```groovy
task androidJavadocs(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    // https://www.2cto.com/kf/201701/582969.html
    // 加上encoding，如果文件中出现中文注释也可以生成javadoc,默认使用UTF-8
    options.encoding = getJavaDocFileEncoding()
    options.charSet = "UTF-8"
    options.author = true
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
}
```