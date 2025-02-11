# Blootgestelde klassen

Notitie
----

### Eigenschappen en methoden
```cpp
class NoteApi {
    Q_PROPERTY(int id)
    Q_PROPERTY(QString name)
    Q_PROPERTY(QString fileName)
    Q_PROPERTY(QString fullNoteFilePath)
    Q_PROPERTY(QString fullNoteFileDirPath)
    Q_PROPERTY(QString relativeNoteFileDirPath)
    Q_PROPERTY(int noteSubFolderId)
    Q_PROPERTY(QString noteText)
    Q_PROPERTY(QString decryptedNoteText)
    Q_PROPERTY(bool hasDirtyData)
    Q_PROPERTY(QQmlListProperty<TagApi> tags)
    Q_PROPERTY(QDateTime fileCreated)
    Q_PROPERTY(QDateTime fileLastModified)
    Q_INVOKABLE QStringList tagNames()
    Q_INVOKABLE bool addTag(QString tagName)
    Q_INVOKABLE bool removeTag(QString tagName)
    Q_INVOKABLE bool renameNoteFile(QString newName)
    Q_INVOKABLE QString toMarkdownHtml(bool forExport = true)
    Q_INVOKABLE QString getFileURLFromFileName(QString localFileName)
    Q_INVOKABLE bool allowDifferentFileName()
};
```

U kunt de methoden van [Datum](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) gebruiken om met `fileCreated` of `fileLastModified` te werken.

### Voorbeeld
```js
script.log (note.fileCreated.toISOString());
script.log (note.fileLastModified.getFullYear());

// hernoemt een notitie naar "nieuwe naam.md"
note.renameNoteFile ("nieuwe naam");

// controleer of het is toegestaan om een andere notitiebestandsnaam te hebben dan de kop
script.log (note.allowDifferentFileName());
```

NoteSubFolder
----

### Eigenschappen en methoden
```cpp
class NoteSubFolderApi {
    Q_PROPERTY(int id)
    Q_PROPERTY(QString name)
    Q_PROPERTY(QQmlListProperty<NoteApi> notes)
    Q_INVOKABLE static NoteSubFolderApi *fetchNoteSubFolderById(int id);
    Q_INVOKABLE static NoteSubFolderApi *activeNoteSubFolder();
    Q_INVOKABLE static QList<QObject*> fetchNoteSubFoldersByParentId(int parentId);
    Q_INVOKABLE QString relativePath();
    Q_INVOKABLE QString fullPath();
};
```

### Voorbeeld
```js
var noteSubFolderQmlObj = Qt.createQmlObject("import QOwnNotesTypes 1.0; NoteSubFolder{}", mainWindow, "noteSubFolder");

// print all subfolder names
noteSubFolderQmlObj.fetchNoteSubFoldersByParentId(parentId).forEach(function(nsf) {
    script.log(nsf.name);
});

// get the active note subfolder
var noteSubFolder = noteSubFolderQmlObj.activeNoteSubFolder();

// print the full and relative path of the active note subfolder
script.log(noteSubFolder.fullPath());
script.log(noteSubFolder.relativePath());

script.log(noteSubFolder.id);
script.log(noteSubFolder.name);

// iterate through notes in note subfolder
for (var idx in noteSubFolder.notes) {
    var note = noteSubFolder.notes[idx];
}
```

Tag
---

### Eigenschappen en methoden
```cpp
klasse TagApi {
     Q_PROPERTY (int id)
     Q_PROPERTY (QString-naam)
     Q_PROPERTY (int parentId)
     Q_INVOKABLE TagApi fetchByName (const QString &name, int parentId = 0)
     Q_INVOKABLE QStringList getParentTagNames ()
};
```

MainWindow
----------

### Eigenschappen en methoden
```cpp
class MainWindow {
    Q_INVOKABLE void reloadTagTree();
    Q_INVOKABLE void reloadNoteSubFolderTree();
    Q_INVOKABLE void buildNotesIndexAndLoadNoteDirectoryList(
            bool forceBuild = false, bool forceLoad = false);
    Q_INVOKABLE void focusNoteTextEdit();
    // Maakt een nieuwe submap voor notities in de huidige submap
    Q_INVOKABLE bool createNewNoteSubFolder(QString folderName = "");
    // Voegt html in de huidige notitie in als markdown
    // Deze methode downloadt ook afbeeldingen op afstand en transformeert "data: image"
    // urls naar lokale afbeeldingen die zijn opgeslagen in de mediamap
    Q_INVOKABLE void insertHtmlAsMarkdownIntoCurrentNote(QString html);
    // Herlaadt de huidige notitie op id
    // Dit is handig als het pad of de bestandsnaam van de huidige notitie is gewijzigd
    Q_INVOKABLE void reloadCurrentNoteByNoteId();
    // Retourneert de lijst met werkruimte-UUID's
    Q_INVOKABLE QStringList getWorkspaceUuidList();
    // Retourneert de UUID van een werkruimte, waarbij de naam van de werkruimte wordt doorgegeven
    Q_INVOKABLE QString getWorkspaceUuid(const QString &workspaceName);
    // Stelt de huidige werkruimte in op UUID
    Q_INVOKABLE void setCurrentWorkspace(const QString &uuid);
};
```

### Voorbeeld
```js
// Forceer een herladen van de notitielijst
mainWindow.buildNotesIndexAndLoadNoteDirectoryList(true, true);

// Creëert een nieuwe notitie submap "Mijn mooie map" in de huidige submap
mainWindow.createNewNoteSubFolder("My fancy folder");

// Voegt html in de huidige notitie in als markdown
mainWindow.insertHtmlAsMarkdownIntoCurrentNote("<h2>my headline</h2>some text");

// Stel de werkruimte 'Bewerken' in als de huidige werkruimte
mainWindow.setCurrentWorkspace(mainWindow.getWorkspaceUuid("Edit"))
```
