Test Skatteverkets Kontrolluppgift XSL 2.0
==========================================

Projektet använder Maven och Saxon 9.7.0.

Testfiler tagna från https://www.skatteverket.se/download/18.361dc8c15312eff6fd17dc/1456496147787/Bilaga4_Exempelfiler_ta_fram_XML_2.0.pdf 
(med tillägg av ku:AnstalldFrom och ku:AnstalldTom i skv_test2.xml).

För att göra transformeringen kör ```mvn xml:transform```. 
Output hamnar i ```target/generated-resources/xml/xslt```.

Fel som uppstår
---------------

##### <, >, <=, >=
Vissa jämförelser mellan innehållet i element använder >= och <= på element som innehåller strängvärden fast jämförelsen borde göras på numeriska värden.
Se testfil ```skv_test2.xml```.

Ex:
```
<xsl:when test="not(ku:AnstalldFrom) or not(ku:AnstalldTom) or ku:AnstalldTom &gt;= ku:AnstalldFrom"/>
borde vara
<xsl:when test="not(ku:AnstalldFrom) or not(ku:AnstalldTom) or numeric(ku:AnstalldTom) &gt;= numeric(ku:AnstalldFrom)"/>
```

##### Kontroll av tillåtna fält vid borttag (205)
Kontrollen verkar inte fungera korrekt. Se testfil ```skv_test3.xml``` (13 Exempelfil kontrolluppgifter med borttagsmarkering (KU10)).
Endast tillåtna fält finns men valideringen blir ändå "När fältet &lt;Borttag&gt; (205) är ifyllt får inte några andra fält förutom identifikatorerna (fälten &lt;UppgiftslamnarId&gt;(201), &lt;Inkomstar&gt;(203), &lt;Specifikationsnummer&gt;(570), &lt;Inkomsttagare&gt;(215), &lt;Fodelsetid&gt;(222) och &lt;AnnatIdNr&gt;(224)) vara ifyllda".
