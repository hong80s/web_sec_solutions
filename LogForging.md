# Log Forging Definition
-	https://owasp.org/www-community/attacks/Log_Injection
-	https://owasp.org/www-community/vulnerabilities/CRLF_Injection

# Solutions
## ref
- https://github.com/augustd/owasp-security-logging/wiki/Log-Forging

A key function of any security logging library is to prevent attackers from forging log entries by submitting input containing CRLF characters. To defend against this, we recommend the following approaches for encoding any CRLF characters included in user input being logged:

## Log4j:

Log4j does not support any type of CRLF encoding scheme. We recommend upgrading to Log4j2, which provides native encoding support as described next.

## Log4j2:

For Log4j2, we recommend using the built-in %encode function with the CRLF encoding scheme in your PatternLayout:
`
<PatternLayout pattern="%d{HH:mm:ss.SSS} %marker [%t] %-5level %logger{36} - %encode{%msg}{CRLF}%n"/>
`
## Logback:

Logback does not have a built-in CRLF encoder, so the library provides the CRLFConverter class. First you must initialize it in your configuration:
`
<conversionRule conversionWord="crlf"
                converterClass="org.owasp.security.logging.mask.CRLFConverter" />
`

Then use the %crlf function in the PatternLayout:

`
<layout class="ch.qos.logback.classic.PatternLayout">
    <Pattern>STDOUT %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %crlf(%msg) %n
    </Pattern>
</layout> 
`