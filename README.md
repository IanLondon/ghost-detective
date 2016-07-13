# ghost-detective

The contents of `public/` directory will be pushed to S3 static site. HTTPS doesn't work with S3 basic static sites, so you have to go to the file itself: https://ghost-detective.s3.amazonaws.com/ghoulish_map.html

## Setup

Install `s3cmd` with:
```
sudo pip install s3cmd
```

Then configure s3cmd by entering appropriate keys, etc, using the wizard:
```
s3cmd --configure
```

### S3 Sync Git Hook
Use a git-hook to sync `public/` with S3. Put the following in `<your-cloned-repo>/.git/hooks/pre-push` :
```
#!/bin/bash
errmsg() {
  echo " [x] failed to deploy to s3!"
  exit "${code}"
}
trap errmsg ERR
s3cmd sync --recursive public/ s3://ghost-detective
echo -e " [*] Successfully deployed to S3"

```

Then make it executable with `chmod +x <your-cloned-repo>/.git/hooks/pre-push`
