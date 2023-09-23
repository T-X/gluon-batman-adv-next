include $(TOPDIR)/rules.mk

PKG_NAME:=gluon-mesh-batman-adv-brmldproxy

include $(TOPDIR)/../package/gluon.mk

define Package/gluon-mesh-batman-adv-brmldproxy
  TITLE:=Bridge MLD Proxy for Gluon
  DEPENDS:=+brmldproxy gluon-mesh-batman-adv
endef

define Package/gluon-mesh-batman-adv-brmldproxy/description
	Gluon community wifi mesh firmware framework: Configuration to
	enable brmldproxy in Gluon with batman-adv.

	The configuration is tuned in a way to enable the usage of
	layer 3 IPv6 multicast routers for routable IPv6 multicast
	address ranges. But with a lot smaller MLD overhead
	compared to the filter_membership_reports=false site.conf option.

	If a node has no multicast listener for a routable IPv6
	multicast address then this node will emit no MLD report
	into the mesh. Furthermore, if a node has multiple multicast
	listening hosts for routable IPv6 multicast addresses then the
	node will act in deputy and respond with combined, aggregated
	MLD reports on behalf.

	This package is currently incompatible with a
	filter_membership_reports=true site.conf option.
endef

define Package/gluon-mesh-batman-adv-brmldproxy/conffiles
/etc/config/brmldproxy
endef

$(eval $(call BuildPackageGluon,gluon-mesh-batman-adv-brmldproxy))
