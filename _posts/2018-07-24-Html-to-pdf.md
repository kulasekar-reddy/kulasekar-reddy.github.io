---
layout: post
title: HTML to PDF convertion
description: "How to convert html to pdf in java"
created: 2018-07-24
comments: true
share:    true
tags: [html2pdf,java]
categories: Java
image:
  background: wood.jpg
---

Convertion from Html String to pdf


### Using wkhtmltopdf

#### Wkhtmltopdf must be installed and working on your system.

In ```build.gradle```

```groovy
allprojects {
	repositories {
		maven { url "https://jitpack.io" }
	}
}

dependencies {
	compile 'com.github.jhonnymertz:java-wkhtmltopdf-wrapper:1.1.4-RELEASE'
}
```

#### Usage
```java
Pdf pdf = new Pdf();
pdf.addPageFromString("<html><head><meta charset=\"utf-8\"></head><h1>Müller</h1></html>");
pdf.saveAs("output.pdf");
```


### Using JTidy and flying-saucer

#### No external installation is required

Steps to convert Html to Pdf

1. Convert html to xhtml(proper closing of tags)
2. Convert xhtml to byte array
3. Write converted byte array to .pdf file

In ```build.gradle```

```groovy
compile group: 'org.xhtmlrenderer', name: 'flying-saucer-pdf', version: '9.1.6'
compile group: 'net.sf.jtidy', name: 'jtidy', version: 'r938'
```

#### Usage
```java
StringReader sr = new StringReader("<html><head><meta charset=\"utf-8\"></head><h1>Müller</h1></html>");
Tidy tidy = new Tidy(); //HTML parser and pretty printer.
tidy.setXHTML(true); //true if tidy should output XHTML
tidy.setInputEncoding("utf-8");			// -utf8
tidy.setOutputEncoding("utf-8");
ByteArrayOutputStream bos = new ByteArrayOutputStream();
tidy.parse(sr, bos);
String xhtml = new String(bos.toByteArray());
FileUtils.writeByteArrayToFile(new File("output.pdf"), toPdf(xhtml));
```
```java
private static byte[] toPdf(String html) throws DocumentException, IOException {
final ITextRenderer renderer = new ITextRenderer();
renderer.setDocumentFromString(html);
renderer.layout();
try (ByteArrayOutputStream fos = new ByteArrayOutputStream(html.length())) {
	renderer.createPDF(fos);
	return fos.toByteArray();
}
}
```