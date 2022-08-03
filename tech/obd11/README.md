# OBD Eleven

Search for and use the `OBD Eleven VAG` app. It is the only one with PRO feautures:

[OBD Eleven VAG app](https://play.google.com/store/apps/details?id=com.voltasit.obdeleven) (Curiously the app ID is `com.voltasit.obdeleven`).

The app will constantly nag about using the "New" app, which is useless because of the missing PRO features:

[OBD Eleven Basic](https://play.google.com/store/apps/details?id=com.voltasit.obdeleven.basic) (App ID: `com.voltasit.obdeleven.basic`).

## Get started

Once the app is launched, connect to the OBD dongle.

And then click on the image of the car. (I always forget this, and can't find anything).

## Reset inspection (service) indicator

Connect and choose car.

Then:
- Go into `Control Units`
- Then `Multimedia`
- Chose `Adaptations` from the list
- Search for "inspection"
- Click on `Inspection, time since last inspection`
- Click and hold the green check-mark to program to "0 days".

## EV high voltage battery SOH (state of health)

Connect and choose car.

Then:
- Go into `Gateway`
- Choose `Live data` from the list
- Search for `vehicle`
- Select `Vehicle properties`
- Then click `OK`

Some information is now shown, among others: `Max energy content` which is the maximum usable charge the battery can hold.

On an Enyaq 80 this is 77.000 Wh when new.

A Cupra Born 58 or Id/Skoda 60 it is 58.000 Wh when new.

Anything below that is degradation.

## Cruise control / ACC

[Zusätzliche (Re)Aktivierung des “Tempomat” (OBDeleven Pro)](https://www.born-forum.de/forum/thread/193-m%C3%B6gliche-codierungen-am-cupra-born-ohne-gew%C3%A4hr-auf-eigene-gefahr/?postID=42663#post42663).

Ja das ACC und Kamera ist in der Serienversion(serienmäßigen Front-/Ausweich/Spurhalteassistent) immer das selbe wie in den Version mit adaptiven Tempomaten bis zum Travel Assist. Hier wird auch gerne von der High Version gesprochen. Der Einzige Unterschied sind die EInstellungen in Software/Steuergeräten. Allerdings hab nichts gefunden das es am ID3/Born nachcodiert wurde. Im Enyaq Longcode Leitfaden gibt es einen Punkt zur Aktivierung des Tempomaten:

Zusätzliche (Re)Aktivierung des “Tempomat” (OBDeleven Pro)

STG 13 - Distanzregelung

Lange Codierung

Cruise control mode >> Aktiviert

Das Speichern per grünem Button rechts unten wird zunächst mit Fehlermeldung abgebrochen.

In dem Falle:

links im Menü auf Sicherheitszugriff klicken und hier den vorgeschlagenen Code „20103“ eingeben und mit OK bestätigen.

Nach dem erfolgreichen Sicherheitszugriff, rechts unten mit dem grünen Button erneut speichern (Drücken und halten).
