## Problem Statement 
If you are working a micro-services based project and you have multiple repos in a single workspace. Here is how you can take update from all the repos using a single command. 

## This is to Take Update from All Repos with a Single Command

You can do this by either utilizing Nodejs or Java.

### Using Node

If you have Node.js installed on your machine, here are the steps:

1. Create a file called `update_repos.js` on the root of the workspace.

2. Paste the following code inside.

3. Open git bash from root or any other terminal but make sure you are on the root of your workspace directory and you can execute git command from there.

4. Run this command `node update_repos.js` and it will take update from all the repos.

##### *JAVASCRIPT CODE STARTS HERE*
```
const { exec } = require('child_process');
const path = require('path');

// Define the paths to your repositories
const repoPaths = [
    path.join(__dirname, 'xxx'),
    path.join(__dirname, 'abc'),
    path.join(__dirname, 'xyz'),
    path.join(__dirname, 'abx'),
	path.join(__dirname, 'foobar')
];

const pullRepo = (repoPath) => {
    return new Promise((resolve, reject) => {
        if (require('fs').existsSync(repoPath)) {
            console.log(`Updating repository at ${repoPath}`);
            exec('git pull', { cwd: repoPath }, (error, stdout, stderr) => {
                if (error) {
                    console.error(`Error updating repo at ${repoPath}:`, stderr);
                    reject(error);
                } else {
                    console.log(stdout);
                    resolve(stdout);
                }
            });
        } else {
            console.error(`Directory ${repoPath} does not exist.`);
            reject(new Error(`Directory ${repoPath} does not exist.`));
        }
    });
};

const updateAllRepos = async () => {
    for (const repoPath of repoPaths) {
        try {
            await pullRepo(repoPath);
        } catch (error) {
            console.error(error);
        }
        console.log('---------------------------------------');
    }
    console.log('All repositories updated.');
};

updateAllRepos();

```
##### *JAVASCRIPT CODE ENDS HERE*

### Using Java

1. Create a file called `UpdateRepos.java` on the root of the workspace.

2. Paste the following code 

3. Compile the java program by using this command `javac UpdateRepos.java` this is a one time command until you want to add a new repo in to the list. 

4. Run the java program `java UpdateRepos`

##### *JAVA CODE STARTS HERE*

```
import java.io.File;
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

public class UpdateRepos {

    // Define the paths to your repositories relative to the snapii folder
    private static final List<String> REPO_PATHS = Arrays.asList(
            "abc",
            "xyz",
            "xxx",
            "abcd",
    );

    public static void main(String[] args) {
        File currentDir = new File(System.getProperty("user.dir"));
        for (String repoPath : REPO_PATHS) {
            updateRepo(new File(currentDir, repoPath));
        }
        System.out.println("All repositories updated.");
    }

    private static void updateRepo(File repoDir) {
        if (repoDir.exists() && repoDir.isDirectory()) {
            System.out.println("Updating repository at " + repoDir.getPath());
            ProcessBuilder processBuilder = new ProcessBuilder("git", "pull");
            processBuilder.directory(repoDir);
            try {
                Process process = processBuilder.start();
                int exitCode = process.waitFor();
                if (exitCode == 0) {
                    System.out.println("Successfully updated repository at " + repoDir.getPath());
                } else {
                    System.err.println("Failed to update repository at " + repoDir.getPath());
                }
            } catch (IOException | InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("---------------------------------------");
        } else {
            System.err.println("Directory " + repoDir.getPath() + " does not exist.");
        }
    }
}
```
##### *JAVA CODE ENDS HERE*

*Note: Make sure you update the `repoPaths` to your repo names*

