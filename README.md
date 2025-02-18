<h1>Active Directory Lab 4: Group Policy, Account Lockouts &amp; User Management</h1>

<h2>Project Summary</h2>
<p>
  In this lab, I configure a Group Policy to enforce an account lockout policy and perform some user management tasks.
  I first set the lockout policy via the Group Policy Management Console (GPMC) on DC-1.
  Then, I test the lockout on a domain user (kid.koxi) on Client-1, unlock the account, reset its password,
  and finally enable/disable the account to observe behavior and logs in Event Viewer.
</p>

<ul>
  <li><strong>Key Components:</strong>
    <ul>
      <li>Group Policy Management Console (GPMC) on DC-1</li>
      <li>Account Lockout Policy Settings</li>
      <li>User account (kid.koxi) testing on Client-1</li>
      <li>Active Directory Users and Computers (ADUC)</li>
      <li>Event Viewer logs for auditing</li>
    </ul>
  </li>
  <li><strong>Goal:</strong> Enforce an account lockout policy and demonstrate account management (lockout, password reset, enable/disable) via Group Policy and ADUC.</li>
</ul>

<hr />

<h2>Steps &amp; Screenshots</h2>
<ol>
  <li>
    <strong>Launch Group Policy Management Console (GPMC)</strong><br />
    Log into DC-1 as the Domain Admin (<code>mydomain.com\jane_admin</code>). Open the Run dialog (<code>Win + R</code>), type <code>gpmc.msc</code>, and press Enter.
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Launching gpmc.msc on DC-1" width="600" />
      <br /><em>Figure 4.1 – Opening the Group Policy Management Console on DC-1.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Edit the Default Domain Policy</strong><br />
    In GPMC, navigate to <code>Domains &gt; mydomain.com &gt; Default Domain Policy</code>, right-click it, and select <strong>Edit</strong>.
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Navigating to Default Domain Policy in GPMC" width="600" />
      <br /><em>Figure 4.2 – Accessing the Default Domain Policy.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Configure the Account Lockout Policy</strong><br />
    In the Group Policy Editor, navigate to <code>Computer Configuration &gt; Policies &gt; Windows Settings &gt; Security Settings &gt; Account Policies &gt; Account Lockout Policy</code>.
    <ul>
      <li>Set <strong>Account lockout duration</strong> to <code>30 minutes</code></li>
      <li>Set <strong>Account lockout threshold</strong> to <code>7 invalid logon attempts</code></li>
      <li>Enable <strong>Administrator lockout</strong></li>
      <li>Set <strong>Reset account lockout counter</strong> to <code>10 minutes</code></li>
    </ul>
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Editing Account Lockout Policy settings" width="600" />
      <br /><em>Figure 4.3 – Configuring the Account Lockout Policy.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Confirm Group Policy Changes</strong><br />
    Go to the <strong>Settings</strong> tab of the Default Domain Policy to verify that the new lockout settings are applied.
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Confirmation of Group Policy settings" width="600" />
      <br /><em>Figure 4.4 – Confirming the updated lockout settings in the policy.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Test the Locked-Out Account on Client-1</strong><br />
    From a Remote Desktop session on Client-1, attempt to log in as the user <code>mydomain.com\kid.koxi</code> repeatedly.
    After 7+ failed attempts, the account should be locked out.
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Failed login attempts on Client-1" width="600" />
      <br /><em>Figure 4.5 – Multiple failed login attempts locking out the account.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Unlock the Locked Account via ADUC</strong><br />
    Log back into DC-1 as <code>jane_admin</code> and open <strong>Active Directory Users and Computers</strong> (ADUC). Navigate to the <code>_EMPLOYEES</code> OU, locate <code>kid.koxi</code>, and open its properties. Check the box indicating the account is locked out to unlock it, then click <strong>Apply</strong>.
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Unlocking kid.koxi in ADUC" width="600" />
      <br /><em>Figure 4.6 – Unlocking the locked account for kid.koxi.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Verify the Unlocked Account on Client-1</strong><br />
    Log into Client-1 again as <code>mydomain.com\kid.koxi</code>. Open Command Prompt (or PowerShell) and run <code>whoami</code> to confirm the correct user is logged in.
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Successful login and whoami output on Client-1" width="600" />
      <br /><em>Figure 4.7 – Verification of successful login using whoami.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Password Reset and Account Management</strong><br />
    Back on DC-1 in ADUC, right-click on <code>kid.koxi</code> and select <strong>Reset Password</strong> to set a new password. Additionally, you can use ADUC to disable or re-enable the account:
    <ul>
      <li>Disable the account and verify on Client-1 that login fails.</li>
      <li>Re-enable the account and confirm that login succeeds.</li>
    </ul>
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Disabling and re-enabling the account in ADUC" width="600" />
      <br /><em>Figure 4.8 – Resetting password and toggling account status in ADUC.</em>
    </p>
  </li>
  <br />

  <li>
    <strong>Review Security Logs</strong><br />
    On DC-1, open Event Viewer by running <code>eventvwr.msc</code> from the Start Menu. Under <strong>Windows Logs &gt; Security</strong>, use the <strong>Find</strong> option to search for <code>kid.koxi</code> and review the logs for successful and failed logon attempts. You can also check the logs on Client-1.
    <p align="center">
      <img src="YOUR_IMAGE_URL_HERE" alt="Event Viewer showing security logs for kid.koxi" width="600" />
      <br /><em>Figure 4.9 – Security logs in Event Viewer for kid.koxi.</em>
    </p>
  </li>
</ol>

<hr />

<h2>Conclusion</h2>
<p>
  In this lab, I implemented an account lockout policy via Group Policy on the Domain Controller.
  I verified the policy by locking out a user account, unlocking it via ADUC, resetting the password, and toggling the account's enablement.
  Finally, I reviewed the corresponding security logs in Event Viewer. This hands-on exercise provides practical insight into managing user access and auditing in Active Directory.
</p>
