[link-false-vulns]:     false-vuln.md

[img-check-vulns]:      ../../images/user-guides/vulnerabilities/check-vuln.png
[img-sort-vulns]:       ../../images/user-guides/vulnerabilities/sort-vulns.png
[img-filter-vulns]:     ../../images/user-guides/vulnerabilities/filter-vulns.png
[img-switch-vulns]:     ../../images/user-guides/vulnerabilities/switch-tab-status.png

[glossary-vulnerability]:       ../../glossary-en.md#vulnerability

# Checking Vulnerabilities

You can check [vulnerabilities][glossary-vulnerability] on the *Vulnerabilities* tab of the Wallarm interface.

By default, all vulnerabilities are divided into groups by the risk level. Lists inside the groups are sorted by the vulnerability discovery date.

![!Vulnerabilities tab][img-check-vulns]

Wallarm stores the history of all discovered vulnerabilities and checks them regularly&nbsp;— both the open and closed ones. If a closed vulnerability opens as a result of checking, you will receive a corresponding notification.

Clicking a vulnerability displays its change log.

## Sort the Vulnerabilities by Risk or Date

You can sort the vulnerabilities by the following criteria:
*   Risk:
    *   High first
    *   Low first
*   Date
    *   From latest
    *   From earliest

![!Sorting the vulnerabilities][img-sort-vulns]

You can filter the vulnerabilities by the risk level by pressing one of the following buttons:
*   *All* — display the vulnerabilities from all of the risk level groups
*   *High risk* — display the high risk vulnerabilities
*   *Medium risk* — display the medium risk vulnerabilities
*   *Low risk* — display the low risk vulnerabilities

![!Filtering the vulnerabilities][img-filter-vulns]

## Filter the Active and Closed Vulnerabilities

Click *Active* to see the active vulnerabilities.

Click *closed* to see the closed vulnerabilities.

![!Tabs to filter vulnerabilities][img-switch-vulns]

You can filter the closed vulnerabilities by clicking the following selectors:

* *all*: The list of closed and false vulnerabilities.
* *fixed*: The list fixed vulnerabilities only.
* *false*: The list of false vulnerabilities only.

!!! info "See also"
    [Working with false vulnerabilities][link-false-vulns]
