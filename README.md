# Survive Game - Reverse-Engineering Challenge
An Android puzzle game: enter your nine-digit ID, then tap a hidden sequence of arrows (← → ↑ ↓) derived from that ID to “survive” and reveal your city.

# Development Process
- Decompiled the APK file to extract the source code
- Opened a new project in Android Studio
- Took the 2 activities - Activity Game and Activiti Menu from the decompiled APK
- Identified required resources by examining each Activity’s fields (layouts, drawables, strings) to know which XML and asset files to extract from the APK’s res/ folder.
- Fixed the manifest to fit to the activity game and activity menu
- Added requiered dependecies to the build.gradle file
- Fixed toast duration bug from
  ```
  Toast.makeText(this, "Survived in " + state, 1).show();
to
  ```
  Toast.makeText(this, "Survived in " + state, Toast.LENGTH_LONG).show();
  ```
- Added a valid endpoint in strings.xml and referenced @string/url in activity menu instead of hard-coded incorrect URL.
- Changed the -
  ```
  activity_Menu.startGame(activity_Menu.menu_EDT_id.getText().toString(), data);
  ```
  to
  ```
   activity_Menu.startGame(Objects.requireNonNull(activity_Menu.menu_EDT_id.getText()).toString(), data);
to prevent null pointer exception

# Game Logic
- The ID the user entered is mapped to steps here using %4 to stay in range 0 to 3 :
 ```
for (int i = 0; i < 9; i++) {
  steps[i] = (ID.charAt(i) - '0') % 4;
}
```
0 is left
1 is right
2 is up
3 is down 

- The game compares each step to steps[currentLevel]
- After 9 steps there's a Toast showing if we did not survive or if we got safely to your city.

# Screenshot
https://github.com/user-attachments/assets/db86b988-9d45-4b99-8b08-13f5ffb4e38e







