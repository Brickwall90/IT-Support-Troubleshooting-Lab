Helpdesk Case Study: Department Share Access Failure (Sales)

Overview

This case simulates a real-world IT helpdesk ticket involving Active Directory-based access control, where a user is unable to access a department file share after a password reset. The investigation focuses on distinguishing authentication from authorization issues and resolving group-based permission misconfiguration.

Incident Summary

  User Report:
    “I can log into my computer, but I cannot access the Sales network drive.”
    
  Affected User:
    Mary.Jones
    
  Environment:
    Domain: LAB.LOCAL
    
  File Server: 
    Windows Server (SMB Shares)
  Client: 
    Windows 11 VM

  Authentication: 
    Active Directory
🧪 Initial Assumptions

At first glance, the issue appeared to be related to:
  Password reset
  Authentication failure
  Network connectivity issues

However, initial testing ruled these out.

  Troubleshooting Steps
1. Verified Authentication
  User successfully logged into domain account
  Password reset completed successfully
  No account lockout detected

  Conclusion: Authentication working correctly

2. Verified General Access
    User accessed \\server\General successfully
   Conclusion:
    Network connectivity confirmed
    SMB file sharing functional

3. Tested Sales Share Access

  Attempted access to:
    \\server\Sales
    Result: ❌ Access Denied
<img width="2560" height="1392" alt="image" src="https://github.com/user-attachments/assets/3779e543-14b4-4dff-bc77-2f98356fe02f" />

4. Checked Active Directory Group Membership
    User groups:
    Domain Users
    Employees
    Missing:
    Sales

Key Finding:
  User was not assigned to the required security group for Sales share access.

  Root Cause Analysis

  The issue was not related to authentication or system failure.

  The root cause was missing group-based authorization.

  The Sales share was correctly secured using an AD security group:

  Access is restricted to members of the Sales group only

  Mary Jones was not a member of this group, therefore access was correctly denied by the system.

 Resolution
    Added user Mary.Jones to Sales security group in Active Directory
    User logged out and logged back in to refresh security token
    Retested access to Sales share

 Result: Access restored successfully

 Final Validation:
  Sales : Allowed
  IT : Denied
  General : Allowed
  
Key Concepts Demonstrated:
  Active Directory security groups
  Role-Based Access Control (RBAC)
  NTFS vs Share permission understanding
  Authentication vs Authorization separation
  Helpdesk troubleshooting methodology
  User session token refresh behavior
  
Lessons Learned:
  Password resets are often unrelated to access issues
  Group membership is a primary control mechanism in AD environments
  Always verify access in layers:
    1.Authentication
    2.Network connectivity
    3.Authorization (groups/permissions)
  Effective troubleshooting requires isolating variables rather than guessing
  
