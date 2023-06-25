# Setup auto-build for Java VM with pm2

Go to your project root and run `cd .git/hooks`.
Create the hook file `post-merge` if it doesn't exist, or rename `post-merge.sample` to `post-merge`.

```bash
$ sudo nano post-merge
```

Write the following to the file
```bash
#!/bin/bash

# the work tree.
TARGET="/home/<user>/<project_name>"

BRANCH="<branch of your choice>"

CRT_BRANCH=`git rev-parse --abbrev-ref HEAD`

if [ "$CRT_BRANCH" = "$BRANCH" ];
  then
    echo "Deploying ${BRANCH} branch..."
    cd "$TARGET"
    if gradle build; then
      if pm2 show <name> &> /dev/null; then
        pm2 restart <name>
      else
        pm2 start <name>.sh --name=<name>
      fi
    else
      echo "Gradle build failed. Skipping 'pm2' steps."
    fi
else
 echo "Doing nothing: only the ${BRANCH} branch may be deployed on this server."
fi
```

Then give the necessary perms to run the commands with `chmod +x post-merge`