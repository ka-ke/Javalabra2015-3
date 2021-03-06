# Checkstyle

Koodin testauksen lisäksi koodin luettavuuden ylläpitäminen on tärkeää. Tässä hyvänä apuvälineenä on staattisen analyysin työkalu Checkstyle. ks. [http://checkstyle.sourceforge.net/](http://checkstyle.sourceforge.net/). Kurssilla käytetään *Ohjelmoinnin Perusteista* ja *Ohjelmoinnin Jatkokurssista* tuttua checkstyleä, [koodin laatuvaatimuksien](Koodin-laatuvaatimukset.md) seurannassa.

>Checkstyle is a development tool to help programmers write Java code that adheres to a coding standard. It automates the process of checking Java code to spare humans of this boring (but important) task. This makes it ideal for projects that want to enforce a coding standard.

### Checkstylen tuominen projektiin

Checkstyle on helppo tuoda Maven-projektiin, lisätään se vain **pom.xml** tiedostoon. Jotta Checkstylen raporteista pääsisi helposti tarkastelemaan lähdekoodissa olevaa ongelmakohtaa, on hyvä käyttää Checkstyleä yhdessä [jxr:n](http://maven.apache.org/plugins/maven-jxr-plugin/) kanssa. 

Lisää **pom.xml** tiedostoon
```
<build>
  <plugins>
  ...
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-checkstyle-plugin</artifactId>
      <version>2.13</version>
      <configuration>
        <configLocation>checkstyle.xml</configLocation>
      </configuration>
    </plugin>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-jxr-plugin</artifactId>
      <version>2.4</version>
    </plugin>
  </plugins>
</build>
```

Checkstylen tarkkailemien virheiden joukko on konfiguroitavissa erillisen konfiguraatiotiedoston avulla, luo nyt tätä varten projektiin uusi **XML**-tiedosto _checkstyle.xml_.

Avaa tiedosto ja korvaa tiedoston sisältö tällä: 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE module PUBLIC "-//Puppy Crawl//DTD Check Configuration 1.3//EN" "http://www.puppycrawl.com/dtds/configuration_1_3.dtd">
<module name='Checker'>

    <module name='TreeWalker'>

        <property name='tabWidth' value='4' />

        <!-- Block Checks -->

        <module name='EmptyBlock' />
        <module name='LeftCurly' />
        <module name='NeedBraces' />
        <module name='RightCurly' />
        <module name='AvoidNestedBlocks' />

        <!-- Miscellaneous -->
        <module name='Indentation' />

        <!--- Naming Conventions -->

        <module name='ClassTypeParameterName' />
        <module name='ConstantName' />
        <module name='LocalFinalVariableName' />
        <module name='LocalVariableName' />
        <module name='MemberName' />
        <module name='MethodName' />
        <module name='MethodTypeParameterName' />

        <module name='PackageName'>
            <property name='format' value='^[a-z]+(\.[a-z][a-z0-9]*)*$' />
        </module>

        <module name='ParameterName' />
        <module name='StaticVariableName' />
        <module name='TypeName' />

        <!-- Whitespace -->

        <module name='GenericWhitespace' />
        <module name='EmptyForInitializerPad' />
        <module name='EmptyForIteratorPad' />
        <module name='MethodParamPad' />

        <module name='NoWhitespaceAfter'>
            <property name='tokens' value='BNOT, DEC, DOT, INC, LNOT, UNARY_MINUS, UNARY_PLUS' />
        </module>

        <module name='NoWhitespaceBefore'>
            <property name='tokens' value='SEMI, DOT, POST_DEC, POST_INC' />
            <property name='allowLineBreaks' value='true' />
        </module>

        <module name='ParenPad' />
        <module name='TypecastParenPad' />
        <module name='WhitespaceAfter' />

        <module name='WhitespaceAround'>
            <property name='allowEmptyConstructors' value='true' />
            <property name='allowEmptyMethods' value='true' />
        </module>

    </module>

    <!-- File Length -->

    <module name='FileLength'>
        <property name='max' value='200' />
    </module>

</module>
```
Lisätään aiemmin luotuun _nbactions.xml_-tiedostoon vielä yksi valikko, jolla voi ajaa Checkstylen helposti suoraan Netbeansistä. Lisää tiedostoon alla oleva pätkä. Tallenna ja aja projektille **Clean and Build**. Nyt pitäisi Custom valikossa näkyä Coberturan ja Pitin lisäksi Checkstyle.

```xml
<actions> 
  ...
  <action>
    <actionName>CUSTOM-checkstyle</actionName>
    <displayName>Checkstyle</displayName>
    <goals>
      <goal>jxr:jxr</goal>
      <goal>checkstyle:checkstyle</goal>
    </goals>
  </action>
</actions>
```

Vaihtoehtoisesti Checkstylen voi ajaa komentoriviltä komennolla <code>mvn jxr:jxr checkstyle:checkstyle</code> Sekä komentoriviltä ajettaessa että suoraan Netbeansistä _checkstyle.xml_ on aiemmin luodun konfiguraatiotiedoston nimi ja komennot olettavat että tiedosto sijaitsee projektihakemiston juuressa.

Generoituasi Checkstyle-raportin löydät sen polusta **/target/site/checkstyle.html**.

Lisää Checkstylestä voi lukea [ohtun sivuilta](https://github.com/mluukkai/ohtu2014/blob/master/web/laskari3.md)
