# opitzconsulting blog

Der GitHub-basierte Blog kann unter der URL http://opitzconsulting.github.io/ angeschaut werden. Die zugehörigen Sourcen werden auf GitHub versioniert verwaltet und können unter der URL https://github.com/opitzconsulting/opitzconsulting.github.io frei zugänglich eingesehen werden.

## Git und Jekyll auf dem eigenen Rechner einrichten

Die Installation von Git wird unter anderem sehr gut auch auf der Webseite von GitHub beschrieben: https://help.github.com/articles/set-up-git/

Um anschließend mit dem Blogging via Jekyll durchzustarten hat auch die Jekyll-Webseite eine sehr gute Anleitung: https://jekyllrb.com/docs/installation/

Weitere Dokumentation:

* http://jekyllrb.com/
* https://git-scm.com/

## Fork des Source Codes des Blogs

Mit einem GitHub-Account kann ein Fork des Repositories https://github.com/opitzconsulting/opitzconsulting.github.io in eurem eigenen GitHub-Account erzeugt werden. Bei einem Fork handelt es sich erst einmal um einen Abzug des Source Codes zum Zeitpunkt der Erstellung des Forks. Klickt dazu auf der Repository-Seite auf GitHub auf den "Fork"-Button (oben rechts).

In meinem Fall lautet mein GitHub-Account codescape und demzufolge erhalte ich eine Kopie des Repositories unter https://github.com/codescape/opitzconsulting.github.io. In diesem Repository kann ich nach Belieben Änderungen vornehmen. Möchte ich diese in den OC-Blog zurückführen, so empfiehlt es sich von vorne herein einen neuen Branch (bspw. "new-post-for-agile-retreat") zu erstellen und in diesem zu arbeiten um später einen Pull-Request mit den Änderungen zu stellen.

Der Pull-Request kann von Kollegen betrachtet, ggf. noch nachgebessert und integriert werden. Wir etablieren hier also den normalen Workflow zur Mitarbeit in diversen Open Source Projekten für die Beteiligung an unserem Blog.

## Struktur des Source Codes

Eine Jekyll-basierte Seite enthält einiges als "Framework", welches in der Regel nicht geändert und angepasst werden muss sowie konkrete Inhalte. In unserem Fall stellt sich die Struktur wie folgt dar, wobei ich die relevanten und für das Schreiben von Artikeln interessanten Pfade herausgehoben habe:

* **_authors/ (Autorenprofile, welche jeder Mitarbeiter für sich selbst anlegen und pflegen kann)**
* _includes/ (Code-Snippets, die an anderen Stellen referenziert und wiederverwendet werden)
* _layouts/ (HTML-Code für die auf allen Seite verwendete Grundstruktur)
* **_posts/ (Blogbeiträge, eine Datei je Blog-Beitrag mit Dateinamen nach bestimmtem Muster)**
* _sass/ (SASS-Files für die Erzeugung von Style-Sheets)
* **_topics/ (Kategorien, die in den Beiträgen verwendet und verlinkt werden können)**
* css/ (CSS-File, welches die SASS-Files inkludiert)
* **img/ (Bilddateien, wie Autorenbilder oder Bilder für Blog-Beiträge)**
* .gitignore
* _config.yml (Konfiguration des Jekyll-Blogs)
* about.md
* authors.html
* categories.html
* debug.html (Automatisiert generierte Seite mit Debug-Informationen)
* feed.xml
* index.html
* readme.md

## Mein Autorenprofil erstellen

Hierfür lege ich im Verzeichnis `_authors/` eine neue Datei mit dem üblichen Namensschema `vorname.nachname.md` an. In dieser orientiere ich mich an den bisherigen Profilen wie beispielsweise meiner Datei: https://raw.githubusercontent.com/opitzconsulting/opitzconsulting.github.io/master/_authors/stefan.glase.md

Das Resultat der Inhalte dieser Datei sieht auf dem Blog anschließend wie folgt aus: http://opitzconsulting.github.io/authors/stefan.glase/

## Einen Beitrag erstellen

Hierfür lege ich im Verzeichnis `_posts/` eine neue Datei mit dem üblichen Namensschema `yyyy-mm-tt-titel-des-beitrages.md` an. Wichtig ist das korrekte Format, damit der Beitrag sowohl dem richtigen Datum zugeordnet werden kann als auch eine sprechende und demzufolge gut auffindbare URL bekommt. In diesem Falll nennen wir die Datei `2014-11-17-agile-retreat-v3.md` und erhalten somit auch später eine sprechende URL im Format: http://opitzconsulting.github.io/2014/11/17/agile-retreat-v3/

Texte werden in diesem Blog mit der Auszeichnungssprache Markdown beschrieben. Zum Schreiben der Texte empfiehlt es sich einen Markdown-fähigen Editor zu verwenden, wie z.B. Atom. Alternativ kann man auch live-Editoren, wie z.B. Dillinger oder JBT ausprobieren. Mehr zur Markdown-Syntax erfährt man auf http://daringfireball.net/projects/markdown/. Ein Beitrag besteht dabei aus einem Kopfbereich im sogenannten Front-Matter-Format und einem Textkörper in Markdown-Schreibweise, wie auszugsweise im Folgenden dargestellt:

    ---
    title:  "Agile Retreat v3"
    categories: agilität
    author: stefan.glase
    ---
    
    Am 19.09.2014 trafen sich eine gut gemischte Truppe von knapp 20 Personen (darunter auch erstmals Mitarbeiter unserer Kunden!) 
    zum **3. Agile Retreat** von OPITZ CONSULTING im beschaulichen Ebersberg in der Nähe von München. Vom Gesellschafter über 
    Studenten bis hin zu einer Vertreterin aus dem Bereich HR, ihnen allen war eines gemeinsam: 
    **Das Interesse an agiler Softwareentwicklung.**
    
    ![Stefan berichtet über Scrum mit vielen Teams](/img/posts/2014-11-17/001.jpg)
    
    Nachdem gegen 18 Uhr der Großteil der Teilnehmer seinen Weg durch das verschlafene Oberbayern gefunden hatte und im idyllischen 
    Landgasthaus eingetroffen war, trafen wir uns zu einem ersten Kennenlernen in lockerer Atmosphäre.
    
    In zufälligen Tischgruppen galt es zunächst Fragen zu beantworten. Wichtig dabei: Nur eine Antwort pro Tisch. Also gleich 
    Kopfüber ins Teamwork. Dabei erfuhren wir neben “mit wem wir es zu tun hatten” auch “die schönsten Erlebnisse im Zusammenhang 
    mit agiler Softwareentwicklung” sowie die “größten Lehren” bei eben dieser. Beispielsweise, dass auch bei einem etablierten 
    Scrum-Team in der 16. Retrospektive noch Verbesserungsvorschläge gefunden werden können, die nicht bereits x-Mal thematisiert 
    wurden.
    
    ...

Was wir hier schon sehr gut sehen ist von oben angefangen der Titel des Blog-Beitrages (Zeile 2), die Verlinkung zu bestimmten Themengebieten (Zeile 3), die Verlinkung zum Autor (Zeile 4) durch Entsprechung des Namens mit dem Namen im Autorenprofil. Im weiteren Inhalt sehen wir den "normalen" unformatierten Text mit den grundlegenden Markdown-Formatierungsanweisungen, beispielsweise die Fettschreibung (Zeile 8) durch Klammerung mittels `**fett geschriebener Text**`, die Darstellung eines Bildes (Zeile 12) durch Angabe des Bildtitels und der URL zum Bild.

## Bilder bereitstellen und einfügen

Bilder für Artikel legen wir im Ordner `img/posts/` ab. Hierbei wird im ersten Wurf davon ausgegangen, dass wir pro Tag nicht mehr als einen Beitrag haben und entsprechend keine Konflikte durch doppelte Namen entstehen. Um dieses Risiko aber weiter zu minimieren können wir den Bildern auch einen zum Artikel passenden Prefix geben. Schreibe ich also am `26.04.2016` einen Artikel zum Thema "Deployment mit Tomcat" kann ich das erste Bild unter dem Pfad `img/posts/2016-04-26/tomcat-001.png` ablegen.

Bilder sollten um Bandbreiten insbesondere auf mobilen Geräten nicht unnötig zu belasten eine bestimmte Dateigröße nicht überschreiten. Dazu wollen wir die Auflösung von Bildern auf maximal 900 Pixel in der Breite und 700 Pixel in der Höhe beschränken. Die Dateigröße sollte einen Maximalwert von 300 kb nicht überschreiten. Dies sind allerdings weiche Vorgaben und wir können immer noch von Fall zu Fall größere Bilder zulassen sofern sich dies begründen lässt. Gespeichert werden Fotos idealerweise als JPEG/JPG mit einer entsprechenden Komprimierung, schematische Darstellungen und Screenshots werden vorzugsweise als PNG gespeichert.

## Code darstellen

Jekyll unterstützt das Highlighting von Code durch eine einfache Klammerung mit der folgenden Syntax:

    {% highlight java %}
    public class HelloWorld {
      public static void main(String[] args) {
        System.out.println("Hello, World");
      }
    }
    {% endhighlight %}

Anstelle von Java können auch diverse andere Sprachen dargestellt werden. Das von GitHub verwendete Plugin Rouge unterstützt die folgenden Sprachen: https://github.com/jneen/rouge/tree/master/lib/rouge/lexers

## Änderungen zur Publikation im Blog bereitstellen

Der übliche Workflow sieht im Git-Sprache wie folgt aus nachdem man einmalig einen Fork erstellt hat:

### Fork aktualisieren

    // Remote des Original Repository ergänzen
    git remote add upstream git://github.com/wouterla/GildedRose.git
 
    // Aktuellen lokalen Master-Branch aktualisieren
    git fetch origin -v
    git fetch upstream -v
    git merge upstream/master
  
    // Änderungen in das eigene Remote-Repository übertragen
    git push origin master

Damit nicht jedes Mal, wenn man seinen Fork aktualisieren möchte, die Eingabe der oben dargestellten Befehle erforderlich ist, kann dafür ein Alias in der Datei `~/.gitconfig` anlegt werden:

    [alias]
        pu = !"git fetch origin -v; git fetch upstream -v; git merge upstream/master"

Auf diese Weise kann mittels `git pu` diese Abfolge von Befehlen einfach ausgeführt werden.

### Neuen Branch erzeugen

    git checkout -b "mein-neuer-artikel"

### Blog weiterentwickeln

Hier werden zum Beispiel mittels `git commit -am "Foto hinzugefügt"` fleißig Änderungen am Blog in eurem lokalen Branch vorgenommen und festgeschrieben.

### Branch in den Remote-Branch auf GitHub pushen

Am Beispiel des Branches "mein-neuer-artikel":

    git push origin mein-neuer-artikel

### Pull-Request eröffnen

Hierfür geht ihr auf die GitHub-Webseite und wählt im Dropdown euren neuen Branch, in diesem Falle "mein-neuer-artikel" aus und klickt im Anschluss auf `New pull request`. Alles Weitere erklärt sich hoffentlich von selbst und ansonsten gerne auf die unten genannten Ansprechpartner zugehen!

### "Freigabe-Prozess"

Im ersten Aufschlag wollen wir einen leichtgewichtien Freigabe-Prozess vorsehen und diesen selbstverständlich nach den ersten Beiträgen hinterfragen und ggf. weiter anpassen und/oder verwerfen. Dieser Freigabeprozess sieht vor, dass sich mindestens ein Kollege den Pull-Request angeschaut und sein Feedback bzw. seine Freigabe für den Artikel beigesteuert hat. Im Anschluss kann der Pull-Request akzeptiert und geschlossen werden.

## Ansprechpartner für weitere Fragen?

Melde dich bei @rattermeyer oder @codescape und wir helfen dir gerne weiter! Oder schau in unseren Slack-Chat rein: https://opitz.slack.com/messages/oc-github-blog/
