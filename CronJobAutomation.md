# ⚙️ Homelab Automation with Cron Jobs

**“A detailed guide to scheduling automated tasks on macOS using cron, the silent, reliable robot assistant for your server.”**

---

## 🧠 What is a Cron Job?

**Cron** is a time-based job scheduler in Unix-like operating systems, including macOS. Think of it as a background service that wakes up every minute to check a special file called a `crontab`. If it finds any tasks scheduled for that exact minute, it runs them automatically.

A **cron job** is a single scheduled task. It's the perfect tool for automating routine maintenance, backups, and scripts without any manual intervention. As long as your Mac is on, cron will execute your commands at the precise moments you define.

---

## 🤝 The Cron & Shell Script Partnership

Cron and shell scripts (`.sh` files) are a powerful duo. Their roles are distinct but complementary:

| Component       | Role                                                                                                                                              |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Shell Script** | **The Action**. A `.sh` file contains the sequence of commands to be executed. For example, a script to `clean up old files` or `backup a database`. |
| **Cron Job**    | **The Schedule**. Cron's job is to run that shell script at a specific time (e.g., "every Sunday at 3 AM"). It doesn't know *what* the script does, only *when* to run it. |

In short: **you write the *what* in a shell script, and you define the *when* in a cron job.**

---

## 🔬 Anatomy of a Cron Job Schedule

The most important part of a cron job is its schedule. It's defined by five fields at the beginning of the line, representing time, followed by the command to run.

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday)
│ │ │ │ │
* * * * *  /path/to/your/command
```

**Special Characters:**

| Character | Meaning                                   | Example                  |
| :-------- | :---------------------------------------- | :----------------------- |
| `*`       | "Every" (e.g., every minute, every hour)  | `* * * * *` (every minute) |
| `,`       | "And" (specifies multiple values)         | `0 8,17 * * *` (at 8 AM and 5 PM) |
| `-`       | "Range" (specifies a range of values)     | `0 9-17 * * 1-5` (9 AM to 5 PM, Mon-Fri) |
| `/`       | "Step" (specifies intervals)              | `*/15 * * * *` (every 15 minutes) |

---

## 🛠️ How to Manage Cron Jobs (The Commands)

You manage cron jobs using the `crontab` command in your terminal.

#### 1. **Edit or Create Cron Jobs**

This is the primary command you'll use. It opens your personal crontab file in a text editor.

```bash
crontab -e
```

> **💡 Pro Tip: Forcing a Specific Editor**
>
> By default, `crontab -e` might open in a confusing editor like `vim`. To guarantee it opens in `nano` (a much simpler editor), you can prefix the command like this:
>
> ```bash
> EDITOR=nano crontab -e
> ```
> This tells the system to use `nano` as the editor for this one command only, saving you from getting stuck.

#### 2. **List (View) Active Cron Jobs**

To see what jobs are currently scheduled without opening the editor:

```bash
crontab -l
```

#### 3. **Remove ALL Cron Jobs**

⚠️ **Use this with extreme caution!** This command will instantly delete your entire crontab file without any confirmation prompt.

```bash
crontab -r
```

To remove a single job, it is much safer to use `crontab -e`, delete the specific line, and then save the file.

---

## 💡 A Practical Homelab Example: Cleaning Up Downloads

Your homelab documentation mentions a routine task: "Clear `/Downloads/Completed` regularly." This is a perfect candidate for automation.

### Part 1: The Shell Script (The *Action*)

First, we create a script that finds and deletes files in the `Completed` directory that are older than 7 days.

1.  Create the script file and make it executable:
    ```bash
    touch scripts/cleanup-downloads.sh
    chmod +x scripts/cleanup-downloads.sh
    ```

    > #### 🤔 What does `chmod +x` do?
    >
    > *   **`chmod`** stands for **"change mode"**. It's a command that changes the permissions of a file.
    > *   **`+x`** means **"add the execute permission"**.
    >
    > By default, a new text file is not considered a program that the system can run. This command tells macOS, "Treat this `.sh` file as an executable script, not just a plain text document." It's a necessary step before cron (or you) can run the script.

2.  Add the following content to `scripts/cleanup-downloads.sh`:

    ```bash
    #!/bin/bash
    
    # --- Configuration ---
    # IMPORTANT: Use the full, absolute path to your downloads folder.
    COMPLETED_DIR="/Users/rajin/Media/Downloads/Completed"
    LOG_FILE="/Users/rajin/cron_homelab.log"
    DAYS_TO_KEEP=7
    
    # --- Script Logic ---
    echo "--- Starting Cleanup on $(date) ---" >> "$LOG_FILE"
    
    # Find and delete files older than DAYS_TO_KEEP days.
    # The "-type f" ensures we only target files, not directories.
    # The "-mtime +$DAYS_TO_KEEP" finds files modified more than 7 days ago.
    # The "-delete" action removes them.
    find "$COMPLETED_DIR" -type f -mtime +$DAYS_TO_KEEP -delete -print >> "$LOG_FILE"
    
    echo "--- Cleanup Finished on $(date) ---" >> "$LOG_FILE"
    
    # To test without deleting, comment out the line above and uncomment the line below:
    # find "$COMPLETED_DIR" -type f -mtime +$DAYS_TO_KEEP -print >> "$LOG_FILE"
    ```

### Part 2: The Cron Job (The *Schedule*)

Now, let's schedule this script to run every Sunday at 4:00 AM.

1.  Open your crontab:
    ```bash
    EDITOR=nano crontab -e
    ```

2.  Add the following line at the bottom of the file. **Remember to use the absolute path to your script.**

    ```crontab
    # At 4:00 AM every Sunday, clean up the completed downloads directory.
    0 4 * * 0 /Users/rajin/path/to/homelab-docs/scripts/cleanup-downloads.sh
    ```
    *(Note: `0` for the day of the week means Sunday)*

3.  Save and exit. Your cleanup task is now fully automated!

---

## 🪵 The Importance of Logging

Cron jobs run silently in the background. If something goes wrong, you won't know unless you check. **Logging is your best friend for debugging.**

The command `>> ~/cron.log 2>&1` is the standard way to capture all output.

*   `>> ~/cron.log`: Appends the standard output (successful messages) of your script to a file named `cron.log` in your home directory.
*   `2>&1`: Redirects the standard error (`2`) to the same place as the standard output (`1`). This ensures that error messages are also written to your log file.

**Example with logging:**
```crontab
0 4 * * 0 /Users/rajin/path/to/homelab-docs/scripts/cleanup-downloads.sh >> /Users/rajin/cron_homelab.log 2>&1
```

---

## ✅ Best Practices & macOS Specifics

*   **Always Use Absolute Paths:** Cron jobs have a very limited environment. They don't know where your files are. Always use the full path to your scripts and any files they interact with (e.g., `/Users/rajin/Media/...` instead of `~/Media/...`).
*   **Make Scripts Executable:** Always run `chmod +x your-script.sh` on your scripts.
*   **Environment Variables:** Cron jobs do not load your `.bash_profile` or `.zshrc`. If your script depends on environment variables, define them at the top of the script itself (like `LOG_FILE` in our example) or source your profile with `source ~/.zshrc && ...`.
*   **SSH and Git:** For scripts that use `git push`, ensure your SSH key is added to the macOS Keychain (`ssh-add --apple-use-keychain`) so it can be used without a passphrase prompt.
*   **macOS Security (Full Disk Access):** If your cron job needs to access protected directories like `Documents` or `Desktop`, you may need to grant "Full Disk Access" to `cron`. Go to `System Settings > Privacy & Security > Full Disk Access`, click the `+` button, press `Cmd + Shift + G`, and navigate to `/usr/sbin/cron` to add it.