# Releases & Versionsverwaltung

**Source:** `RAW/02_Change Management` → [02 Change Management](02_Change_Management.md)

## Namenskonvention

- Format: `R<YYYY>.<MM>` (z.B. R2024.07)
- Patches: `R<YYYY>.<MM>_Patch<YYYYMMDD>`

## Release-Typen

| Typ | Kadenz | Inhalt |
|-----|--------|--------|
| **Major Release** | 6 pro Jahr (alle zwei Monate) | Neue Features, Erweiterungen |
| **Patch/Hotfix** | Zwischen Releases, nach Bedarf | Kritische Fixes |

> **Kein Minor Release.** CH/FTF folgt strikt den Major Releases. In Ausnahmefällen kann das Team davon abweichen, aber der Standard ist: Major oder Patch.

## Zertifizierungsprozess

1. Entwicklung abgeschlossen
2. Testing bestanden (alle Stufen)
3. Business-Freigabe (UAT-Zertifikat)
4. Produktionsabnahme (PAV-Zertifikat)
5. Produktivsetzung + Release Bulletin
