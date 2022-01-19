# killbill-oss-parent
![Maven Central](https://img.shields.io/maven-central/v/org.kill-bill.billing/killbill-oss-parent?color=blue&label=Maven%20Central)

Kill Bill OSS Parent: base pom for the various Kill Bill projects.

## Kill Bill compatibility

| OSS parent version | Kill Bill version |
| -----------------: | ----------------: |
| 0.94.y             | 0.16.z            |
| 0.140.y            | 0.18.z            |
| 0.141.y            | 0.19.z            |
| 0.142.y            | 0.20.z            |
| 0.143.y            | 0.22.z            |
| 0.144.y            | 0.22.z            |
| 0.145.y            | 0.23.z            |

We've upgraded numerous dependencies in 0.144.x (required for Java 11 support).

## Usage

Add Kill Bill OSS Parent as the parent to a project:

```xml
<parent>
  <groupId>org.kill-bill.billing</groupId>
  <artifactId>killbill-oss-parent</artifactId>
  <version>... release version ...</version>
</parent>
```

## About

Kill Bill is the leading Open-Source Subscription Billing & Payments Platform. For more information about the project, go to https://killbill.io/.
