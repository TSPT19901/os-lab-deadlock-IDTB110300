# os-lab-deadlock-IDTB110300

## Student Info

- Name: Thon Sopheaktra
- Student ID: IDTB110300

# Level 1: Virtual Vault Provisioning (Formatting & Mounting)

### Screenshot

(Insert df -h | grep loop screenshot)
![alt text](<Level 1.png>)

### Explanation

You can see from the output that both vault image files got picked up as loop devices
and are now mounted and accessible in the file system. Pretty much confirms the setup worked.

# Level 2 & 3: The Naive Sync (Introducing the Flaw) & The Local Circular Wait (Triggering Deadlock)

### Screenshot

(Insert frozen terminals)
![alt text](<Level 3 sync_up.png>)
![alt text](<Level 3 sync_down.png>)
![alt text](<Level 3 check frozen.png>)

### Explanation

Both scripts end up stuck waiting on each other. sync_up grabs Alpha and sits there
waiting for Beta, but sync_down already grabbed Beta and is waiting for Alpha.
Neither one will let go, so they just freeze forever.

# Level 4: Site-to-Site Sync (Multiplayer Deadlock)

### Screenshot

(Insert multiplayer deadlock)
![alt text](<Level 4 First.png>)
![alt text](<Level 4.png>)

### Explanation

Both players grab their own vault lock and then reach over to lock their
partner's vault. But since both do this at the same time, they end up
waiting on each other across the server — completely frozen.s.

# Level 5: Global Resource Ordering (The Patch)

### Screenshot

(Insert successful execution)
![alt text](<Level 5.png>)

### Explanation

The fix was simple — just make everyone grab Alpha first, no matter what.
Once both scripts follow the same order, there's no way for them to end up
waiting on each other.

# Level 6: Deadlock Recovery (The Timeout Patch)

### Screenshot

(Insert timeout error)
![alt text](<level 61.png>)
![alt text](<Level 62.png>)

### Explanation

Instead of waiting forever, the script just gives up after 5 seconds if it
can't get the lock. It prints an error and exits cleanly, which is way better
than freezing the whole terminal.

# Level 7: Safe Ejection (Teardown)

### Screenshot

(Insert clean df -h output)
![alt text](<Level 7.png>)

### Explanation

If you just delete the image files without unmounting first, things can get
messy fast. The teardown script makes sure everything is properly detached
before cleaning up, so nothing gets corrupted or left hanging in the kernel.
