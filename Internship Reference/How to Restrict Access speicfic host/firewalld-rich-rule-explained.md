# 🔥 Firewalld Rich Rule Explanation

### Command

``` bash
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.1.30.22/23" port port="22" protocol="tcp" accept'
```

------------------------------------------------------------------------

## 🧱 Step-by-Step Breakdown

  ----------------------------------------------------------------------------------------
  Part                                   Description
  -------------------------------------- -------------------------------------------------
  **`sudo`**                             Runs the command with administrative privileges.

  **`firewall-cmd`**                     Command-line tool to manage the **firewalld**
                                         service, which controls network traffic.

  **`--permanent`**                      Makes the rule **persistent** across reboots (not
                                         temporary).

  **`--add-rich-rule`**                  Adds a **rich rule**, allowing for advanced
                                         filtering conditions.

  **`rule family="ipv4"`**               Specifies that this rule applies to **IPv4**
                                         traffic.

  **`source address="10.1.30.22/23"`**   Limits the rule to traffic **originating from the
                                         10.1.30.22/23 network** (CIDR range).

  **`port port="22" protocol="tcp"`**    Targets **TCP port 22**, which is used by
                                         **SSH**.

  **`accept`**                           Action to **allow** (accept) traffic that matches
                                         the rule.
  ----------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 🧩 Meaning

This command **permanently allows SSH (TCP port 22) traffic** from any
host within the **10.1.30.22/23** IPv4 network range.

------------------------------------------------------------------------

## 🚫 To Block Instead

### Reject traffic (send error message)

``` bash
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.1.30.22/23" port port="22" protocol="tcp" reject'
```

### Drop traffic (silent block)

``` bash
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.1.30.22/23" port port="22" protocol="tcp" drop'
```

------------------------------------------------------------------------

## ⚙️ Apply Changes Immediately

After adding a **permanent** rule, reload `firewalld` to apply it
without restarting:

``` bash
sudo firewall-cmd --reload
```

------------------------------------------------------------------------

## ✅ Summary

> Permanently allow or block SSH (port 22/TCP) from specific IPv4
> sources using `firewalld` rich rules.\
> Remember to reload `firewalld` after making changes to activate new
> rules.
