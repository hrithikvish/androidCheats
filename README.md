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

12. Binding in Fragments
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        binding = FragmentRoomBinding.inflate(inflater, container, false);
        return binding.getRoot();
    }
