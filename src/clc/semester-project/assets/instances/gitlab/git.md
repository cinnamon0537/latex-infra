# GitLab – Clone, Commit und Push Test (Milestone 2)

## 1. Repository über Public IP clonen
Das Repository wurde nicht über die interne DNS-Adresse geklont, sondern über die öffentliche IP-Adresse des GitLab Servers, da der lokale Rechner sich nicht im AWS-VPC befindet.

git clone http://<PUBLIC_IP>/root/test-runner.git

Beispiel:
git clone http://34.205.xxx.xxx/root/test-runner.git

Im Anschluss wurden Benutzername und Passwort des GitLab-Accounts eingegeben.

---

## 2. Ins Projektverzeichnis wechseln
cd test-runner

---

## 3. Testdatei erzeugen
echo "test run $(date)" >> demo.txt

---

## 4. Datei zum Commit vormerken
git add .

---

## 5. Commit erstellen
git commit -m "demo pipeline test"

---

## 6. Änderungen ins GitLab Repository pushen
git push

Beim Push wurden Benutzername und Passwort von GitLab abgefragt.

---

## 7. Pipeline prüfen
Nach dem Push wurde die Pipeline im GitLab Web UI automatisch ausgelöst:
- CI/CD → Pipelines
- expected result:
  - pending
  - running
  - passed

---

## Ergebnis
Damit wurde erfolgreich demonstriert, dass:
- von einem lokalen Rechner ein GitLab Repository geklont werden kann
- Änderungen committet und gepusht werden können
- der GitLab Runner korrekt registriert ist
- die Pipeline automatisch gestartet wird

Dieser Schritt erfüllt die Anforderungen des Milestone-2-Abschnitts „GitLab Server & Runner“.
