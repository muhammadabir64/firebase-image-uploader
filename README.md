# firebase-image-uploader
```javascript
const config = {
  apiKey: "",
  authDomain: "",
  projectId: "",
  storageBucket: "",
  messagingSenderId: "",
  appId: "",
  databaseURL: ""
};
const app = firebase.initializeApp(config);

const progress_bar = document.getElementById("progress_bar");
const file = document.getElementById("file");
const filename_generator = (Math.random() + 1).toString(36).substring(7);

file.addEventListener("change", function(e){
  let file = e.target.files[0];
  let storageRef = firebase.storage().ref(filename_generator+file.name);
  let task = storageRef.put(file);
  task.on("state_changed", function progress(snapshot){
    let percentage = (snapshot.bytesTransferred/snapshot.totalBytes)*100;
    progress_bar.innerText = Math.round(percentage)+"%";
    progress_bar.style.width = Math.round(percentage)+"%";
  }, function error(error){
    console.log(error);
  }, function complete(){
    task.snapshot.ref.getDownloadURL().then((url) => {
      document.getElementById("img_view_div").classList.remove("d-none");
      document.getElementById("img_viewer").src = url;
      document.getElementById("img_url").href = url;
      document.getElementById("img_url").innerText = url;
      console.log(url);
    });
  });
});
```
## set firebase storage rules
```
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```
![Untitled design](https://user-images.githubusercontent.com/51321911/183974101-674b4048-f6a5-41ee-b3c4-89c6f2dbcdd6.gif)
