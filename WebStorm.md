# WebStorm Settings
                                
## Opening Projects
WebStorm works better when the projects are opened
individually. Using the repo root is slower: renames,
auto-complete, etc; and import paths don't resolve perfectly.

## Live Static Analysis
- _Preferences  → Language & Frameworks  → JS  → Code Quality Tools_
- Enable `eslint`
- Select **Manual ESLint configuration**
- In Ubuntu **ESLint package:** `~/lib/node_modules/eslint`
    - In the Setup.md eslint was installed globally.
- **Configuration file:** Point it to an .eslintrc.json
  
## Style Conventions
- _Preferences → Version Control → Commit Dialog → Before Commit_ 
   - Uncheck _Optimize Imports_
   

## Windows Defender Exclusions
- Windows Security → Virus & Threat Protection → Manage Settings → Exclusions
    - ~/.Webstorm2019
    - $UXREPO


## Typing `º` (Degree Symbol) in Windows/Linux
```shell script
cat << EOF > ~/.config/JetBrains/WebStorm2021.2/options/macros.xml
<application>
  <component name="ActionMacroManager">
    <macro name="InsertDegree">
      <typing text-keycode="186:0">º</typing>
    </macro>
  </component>
</application>
EOF
```

Then, in the Keymap Editor, open Macros, and assign `Alt+0` to "InsertDegree".

That's equivalent to typing, with a NumPad, `Alt + 0176`


## GVim or MacVim as External Tool
_Preferences → Tools → External Tools_
- Program: `gvim` or `/opt/homebrew/bin/mvim`
- Arguments: `"$FilePath$"`
- Working Directory: `$ProjectFileDir$`

## SVGO as External Tool
- Program: `svgo`
- Arguments: `"$FilePath$" --pretty --precision=1 --indent=2`
- Working Dir: `$ProjectFileDir$`

