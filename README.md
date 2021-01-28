docker run --network host --name yang-explore -it yang-explore:2 bash
docker run -it --name sysrepo -p 830:830 sysrepo/sysrepo-netopeer2:latest

for i in $(ls yang_files); do  sudo docker cp yang_files/$i  sysrepo:/tmp;  done

docker exec sysrepo sysrepoctl -i /tmp/ieee802-dot1q-types.yang
docker exec sysrepo sysrepoctl -i /tmp/ietf-yang-types@2010-09-24.yang
docker exec sysrepo sysrepoctl -i /tmp/ietf-interfaces@2014-05-08.yang
docker exec sysrepo sysrepoctl -i /tmp/ieee802-types.yang
docker restart sysrepo
docker exec sysrepo sysrepoctl -i /tmp/iana-if-type@2014-05-08.yang
docker restart sysrepo
docker exec sysrepo sysrepoctl -i /tmp/ieee802-dot1q-bridge.yang


NACM problem:
https://github.com/CESNET/netopeer2/issues/658

config-acm.xml:
<nacm xmlns="urn:ietf:params:xml:ns:yang:ietf-netconf-acm">
     <enable-nacm>false</enable-nacm>
</nacm>

sysrepocfg --import=config-acm.xml --datastore startup --module ietf-netconf-acm