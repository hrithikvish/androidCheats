# androidStudioCheats
Random stuffs about Android App Development on Android Studio collected from stack overflow, etc. at one place which i might need in future

1. Make an INVISIBLE View VISIBLE on button click or something - 
    [viewName].setVisibility(View.VISIBLE);

2. Share Button - 
```java
shareBtn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Intent sendIntent = new Intent();
        sendIntent.setAction(Intent.ACTION_SEND);
        sendIntent.putExtra(Intent.EXTRA_TEXT, jokeBox.getText());
        sendIntent.setType("text/plain");
        Intent shareIntent = Intent.createChooser(sendIntent, null);
        startActivity(shareIntent);
    }
});
```
 
3. Copy to Clipboard Button - 
```java
copyBtn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        ClipboardManager clipboard = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
        ClipData clip = ClipData.newPlainText(null, jokeBox.getText());
        clipboard.setPrimaryClip(clip);
        Toast.makeText(MainActivity.this, "Joke Copied", Toast.LENGTH_SHORT).show();
    }
});
```

4. Justify Text - android:justificationMode="inter_word"

5. Word Wrap TextView - 
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"

7. Volley Request Queue -  
      MainActivity File - requestQueue = Volley.newRequestQueue(this);
      Fragment File - requestQueue = Volley.newRequestQueue(requireContext());

8. Image not showing -
      android:src (if youre using something else)

9. Image Not Loading from api url - 
      Add this in Manifest - <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
      Sometimes it may not load because the url is http and not https in that case add this in application tag in android manifest file - 
        android:usesCleartextTraffic="true"

10. Enable BInding 
    in gradle within android - buildFeatures { viewBinding true }

11. Room database Dependency
    implementation "androidx.room:room-runtime:2.5.2"
    annotationProcessor "androidx.room:room-compiler:2.5.2"

12. Close Activity - finish()

13. Fragment Activity Crashing? Add the Button setOnCLickListener code in OnCreateView 

14. Binding in Fragments
```java
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
binding = FragmentRoomBinding.inflate(inflater, container, false);
return binding.getRoot();
}
```

15.     
```java
ArrayList<Note> notesArr = (ArrayList<Note>) databaseHelper.noteDao().getAllNotes();

ArrayList<String> notesString = new ArrayList<>();
for (Note note : notesArr) {
    notesString.add(note.toString());
}

ArrayAdapter<String> arrayAdapter = new ArrayAdapter<>(requireContext(), android.R.layout.simple_list_item_1, notesString);
```

16. Refresh/ Replace Fragment
```java
private void refreshFragment(Fragment fragment) {
    FragmentManager fragmentManager  = getFragmentManager();
    FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
    fragmentTransaction.replace(R.id.frameLayout, fragment);
    fragmentTransaction.commit();
}
```

17. Make imageView appear round using cardView(See example below)

```xml
<androidx.cardview.widget.CardView
    android:id="@+id/cardView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_gravity="center"
    android:layout_marginTop="20dp"
    app:cardCornerRadius="40dp"
    app:cardBackgroundColor="@color/white">
    <ImageView
        android:id="@+id/profilePicture"
        android:layout_width="80dp"
        android:layout_height="80dp"
        tools:srcCompat="@tools:sample/avatars"/>
</androidx.cardview.widget.CardView>
```

18. Auto Capitalize first char in a editText
```kotlin
binding.edtName.addTextChangedListener(object : TextWatcher {
            override fun beforeTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {}
            override fun onTextChanged(p0: CharSequence?, p1: Int, p2: Int, p3: Int) {}
            override fun afterTextChanged(editable: Editable?) {
                if(editable.toString().isNotEmpty()) {
                    val start = editable?.length; // Get current cursor position (end)
                    val defaultName = editable.toString();

                    if (Character.isLowerCase(defaultName[0])) {
                        val firstLetterUppercase = defaultName.substring(0, 1).toUpperCase();
                        val remainingString = defaultName.substring(1);
                        val capitalizedName = firstLetterUppercase + remainingString;

                        editable?.replace(0, editable.length, capitalizedName);

                        Selection.setSelection(editable, start ?: -1);
                    }
                }
            }
        })
```

19. Dismissible SnackBar with rounded corners (corner radius)
```kotlin
val sb = Snackbar.make(
    binding.root,
    "Message String",
    Snackbar.LENGTH_INDEFINITE
).setBackgroundTint(ContextCompat.getColor(requireContext(), R.color.snackbar_bg_yellow))
    .setTextColor(ContextCompat.getColor(requireContext(), R.color.black))
    .setActionTextColor(ContextCompat.getColor(requireContext(), R.color.red))
    .apply {
        view.background = resources.getDrawable(R.drawable.bg_rounded_snackbar, null)
    }
sb.setAction("Dismiss") {
    sb.dismiss()
}
sb.show()
```

20. Share bitmap/image using Intent
```kotlin

val bitmapUri = genBitmapUri(bitmap)
private fun startImageSharingIntent(bitmapUri: Uri?) {
    try {
        val sendIntent = Intent()
        sendIntent.putExtra(Intent.EXTRA_STREAM, bitmapUri)
        sendIntent.setAction(Intent.ACTION_SEND)
        sendIntent.setPackage("com.whatsapp") // optional
        sendIntent.setType("image/png")
        sendIntent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION)
        mContext.startActivity(sendIntent)
        dismiss()
    } catch (e: Exception) {
        Log.d("SHARE-IMG-TAG", "shareImg:" + e.message)
        Toast.makeText(mContext, "App not installed", Toast.LENGTH_SHORT).show()
        dismiss()
    }
}

private fun genBitmapUri(bitmap: Bitmap): Uri {
    val fileName = "temp_image.png"
    val outputFile = File(mContext.externalCacheDir, fileName)
    val outputStream = outputFile.outputStream()

    bitmap.compress(Bitmap.CompressFormat.PNG, 100, outputStream)
    outputStream.flush()
    outputStream.close()

    val bitmapUri = FileProvider.getUriForFile(mContext, "${mContext.packageName}.fileprovider", outputFile)
    return bitmapUri
}
```

21. making custom "Dialog" take full width of the screen
```kotlin
window?.setBackgroundDrawable(ColorDrawable(Color.TRANSPARENT))
window?.setLayout(ConstraintLayout.LayoutParams.MATCH_PARENT, ConstraintLayout.LayoutParams.WRAP_CONTENT)
```

22. take a screenshot of a view as Bitmap
```kotlin
private fun getScreenShotFromView(v: View): Bitmap? {
    var screenshot: Bitmap? = null
    try {
        screenshot = Bitmap.createBitmap(v.measuredWidth, v.measuredHeight, Bitmap.Config.ARGB_8888)
        val canvas = Canvas(screenshot)
        v.draw(canvas)

        if (v is CardView && v.radius > 0) {
            val radius = v.radius
            val output = Bitmap.createBitmap(screenshot.width, screenshot.height, Bitmap.Config.ARGB_8888)
            val outputCanvas = Canvas(output)
            val paint = Paint()
            paint.isAntiAlias = true
            paint.shader = BitmapShader(screenshot, Shader.TileMode.CLAMP, Shader.TileMode.CLAMP)

            outputCanvas.drawColor(Color.TRANSPARENT, PorterDuff.Mode.CLEAR)

            outputCanvas.drawRoundRect(
                RectF(0f, 0f, screenshot.width.toFloat(), screenshot.height.toFloat()),
                radius, radius, paint
            )
            screenshot.recycle()
            return output
        }

    } catch (e: Exception) {
        Log.d("SHARE-IMG-TAG", "Failed because: " + e.message)
    }
    return screenshot
}
```

23. 
