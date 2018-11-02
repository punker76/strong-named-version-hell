# strong-named-version-hell
This repo tries to understand strong naming with the new msbuild sdk csproj format

**MahApps.Metro.IconPacks.OldFormat** uses the old csproj format. It builds a strong named assembly with the Release configuration.

**MahApps.Metro.IconPacks.NewFormat** uses the new csproj format together with the [MSBuild.Sdk.Extras](https://github.com/onovotny/MSBuildSdkExtras) from Oren Novotny [@onovotny](https://github.com/onovotny). It builds also a strong named assembly with the Release configuration.

The **WorkingApp** use the assemblies from `MahApps.Metro.IconPacks.OldFormat` and all works fine.

The **NonWorkingApp** use now the assemblies from **MahApps.Metro.IconPacks.NewFormat**.

Now I get a `System.IO.FileLoadException`:

```
System.IO.FileLoadException occurred
  FileName=MahApps.Metro.IconPacks, Version=0.0.0.0, Culture=neutral, PublicKeyToken=69f1c32f803d307e
  FusionLog==== Zustandsinformationen vor Bindung ===
LOG: DisplayName = MahApps.Metro.IconPacks, Version=0.0.0.0, Culture=neutral, PublicKeyToken=69f1c32f803d307e
 (Fully-specified)
LOG: Appbase = file:///d:/projects/git/strong-named-version-hell/src/NonWorkingApp/WpfApplication26/bin/Debug/
LOG: Ursprünglicher PrivatePath = NULL
Aufruf von Assembly : PresentationFramework, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35.
===
LOG: Diese Bindung startet im default-Load-Kontext.
LOG: Die Anwendungskonfigurationsdatei wird verwendet: d:\projects\git\strong-named-version-hell\src\NonWorkingApp\WpfApplication26\bin\Debug\WpfApplication26.vshost.exe.config
LOG: Die Hostkonfigurationsdatei wird verwendet: 
LOG: Die Computerkonfigurationsdatei von C:\Windows\Microsoft.NET\Framework\v4.0.30319\config\machine.config wird verwendet.
LOG: Verweis nach der Richtlinie: MahApps.Metro.IconPacks, Version=0.0.0.0, Culture=neutral, PublicKeyToken=69f1c32f803d307e
LOG: Download von neuem URL file:///d:/projects/git/strong-named-version-hell/src/NonWorkingApp/WpfApplication26/bin/Debug/MahApps.Metro.IconPacks.DLL.
WRN: Der Vergleich des Assemblynamens führte zum Konflikt: Hauptversion.
ERR: Das Setup der Assembly konnte nicht abgeschlossen werden (hr = 0x80131040). Die Suche wurde beendet.

  HResult=-2146234304
  Message=Die Datei oder Assembly "MahApps.Metro.IconPacks, Version=0.0.0.0, Culture=neutral, PublicKeyToken=69f1c32f803d307e" oder eine Abhängigkeit davon wurde nicht gefunden. Die gefundene Manifestdefinition der Assembly stimmt nicht mit dem Assemblyverweis überein. (Ausnahme von HRESULT: 0x80131040)
  Source=mscorlib
  StackTrace:
       bei System.Reflection.RuntimeAssembly._nLoad(AssemblyName fileName, String codeBase, Evidence assemblySecurity, RuntimeAssembly locationHint, StackCrawlMark& stackMark, IntPtr pPrivHostBinder, Boolean throwOnFileNotFound, Boolean forIntrospection, Boolean suppressSecurityChecks)
       bei System.Reflection.RuntimeAssembly.nLoad(AssemblyName fileName, String codeBase, Evidence assemblySecurity, RuntimeAssembly locationHint, StackCrawlMark& stackMark, IntPtr pPrivHostBinder, Boolean throwOnFileNotFound, Boolean forIntrospection, Boolean suppressSecurityChecks)
  InnerException: 
```

![2018-11-02_13h38_21](https://user-images.githubusercontent.com/658431/47916322-14d15280-dea6-11e8-9abe-6654eb1063b1.png)

**FusionLog**

```
=== Zustandsinformationen vor Bindung ===
LOG: DisplayName = MahApps.Metro.IconPacks, Version=0.0.0.0, Culture=neutral, PublicKeyToken=69f1c32f803d307e
 (Fully-specified)
LOG: Appbase = file:///d:/projects/git/strong-named-version-hell/src/NonWorkingApp/WpfApplication26/bin/Debug/
LOG: Ursprünglicher PrivatePath = NULL
Aufruf von Assembly : PresentationFramework, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35.
===
LOG: Diese Bindung startet im default-Load-Kontext.
LOG: Die Anwendungskonfigurationsdatei wird verwendet: d:\projects\git\strong-named-version-hell\src\NonWorkingApp\WpfApplication26\bin\Debug\WpfApplication26.vshost.exe.config
LOG: Die Hostkonfigurationsdatei wird verwendet: 
LOG: Die Computerkonfigurationsdatei von C:\Windows\Microsoft.NET\Framework\v4.0.30319\config\machine.config wird verwendet.
LOG: Verweis nach der Richtlinie: MahApps.Metro.IconPacks, Version=0.0.0.0, Culture=neutral, PublicKeyToken=69f1c32f803d307e
LOG: Download von neuem URL file:///d:/projects/git/strong-named-version-hell/src/NonWorkingApp/WpfApplication26/bin/Debug/MahApps.Metro.IconPacks.DLL.
WRN: Der Vergleich des Assemblynamens führte zum Konflikt: Hauptversion.
ERR: Das Setup der Assembly konnte nicht abgeschlossen werden (hr = 0x80131040). Die Suche wurde beendet.
```

This exception occures only if I checked the "Enable .NET Framework source stepping", but other users get it also without these setting.

![2018-11-02_13h41_31](https://user-images.githubusercontent.com/658431/47916299-0420dc80-dea6-11e8-8981-75ddcda1ba02.png)

