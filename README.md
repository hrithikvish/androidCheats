# androidStudioCheats
Random stuffs about Android App Development on Android Studio collected from stack overflow, etc. at one place which i might need in future

1. Make an INVISIBLE View VISIBLE on button click or something - 
    [viewName].setVisibility(View.VISIBLE);

2. Share Button - 

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
       
3. Copy to Clipboard Button - 

    copyBtn.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
            ClipboardManager clipboard = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
            ClipData clip = ClipData.newPlainText(null, jokeBox.getText());
            clipboard.setPrimaryClip(clip);
            Toast.makeText(MainActivity.this, "Joke Copied", Toast.LENGTH_SHORT).show();
        }
    });

4. Justify Text - android:justificationMode="inter_word"

5. Word Wrap TextView - 
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"

6. Use JSON data from API Calls into Java - (goto file "JSON in JAVA.txt")
