# App Engine: 3 Ways: Challenge Lab

### Put your message from Task 4
```cmd
export MESSAGE=""
```

### Navigation Menu > Compute Engine > VM instance

### Click ssh downarrow icon > View gcloud command > Run in cloudshell > Paste ` --quiet` with same command 
```cmd
gcloud services enable appengine.googleapis.com
git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git
cd python-docs-samples/appengine/standard_python3/hello_world
exit
```

```cmd
git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git
cd python-docs-samples/appengine/standard_python3/hello_world
gcloud app deploy
sed -i 's/Hello World!/'"$MESSAGE"'/g' main.py
gcloud app deploy
```

### Select the Region from task 3 and press y when asked
