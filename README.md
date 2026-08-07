Two routes:

Quick — override in the configuration. Use the Translate link next to Show Technical Details to maintain the labels for this configuration. Scope: this configuration only.

Robust — own data elements. Create ZWR_DE_HDAY_VALID_FROM / ZWR_DE_HDAY_VALID_TO (type DATS) with proper short and column labels in DE and EN, use them in ZWR_S_HDAY_LOG, and change the two value attributes' ABAP Dictionary Type. Labels then come through automatically everywhere and in every language. Same treatment would fix [OXYGENID] — maintaining field labels on ZWR_DCPT_OXYGENID is not advisable though, since that data element is used elsewhere; override that one in the configuration.

I'd start with Translate; if the labels don't stick or you need them in several languages, switch to the data-element route.

Which do you want? Also, do you want OXYGENID visible to end users, or is it purely technical? That's one of the open points from the requirement and it decides whether row 8 is in or out.
