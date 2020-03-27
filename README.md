# r8101-1.035.02
Repo for r8101-1.035.02 fix for RTL8106EUS mac address problem

The RTL8106EUS reads the wrong Mac address using r8101-1.035.02.
Example: The real mac is 38:63:bb:9e:58:1f , but rtl8101_get_mac_address reads: 38:63:58:1f:58:1f
Bytes 2 and 3 are copying bytes 4 and 5.

This code:
  if (tp->mcfg == CFG_METHOD_14 || tp->mcfg == CFG_METHOD_17 ||
            tp->mcfg == CFG_METHOD_18 || tp->mcfg == CFG_METHOD_19) {
                *(u32*)&mac_addr[0] = rtl8101_eri_read(tp, 0xE0, 4, ERIAR_ExGMAC);
                *(u16*)&mac_addr[2] = rtl8101_eri_read(tp, 0xE4, 2, ERIAR_ExGMAC);
        } 

Seems to be reading the wrong values.
So ignoring CFG_METHOD_17 in there seems to do the trick.
