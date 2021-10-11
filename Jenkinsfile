pipeline {
agent any
stages {
stage ('vnf-onboarding') {
steps {
sh 'osm nfpkg-create slice_demo_vnf'
sh 'osm nfpkg-create slice_demo_vnf2'
sh 'osm nfpkg-40 slice_demo_middle_vnf'
}
}
stage ('ns-onboarding') {
steps {
sh 'osm nspkg-create slice_demo_ns' 
sh 'osm nspkg-create slice_demo_ns2'
sh 'osm nspkg-create slice_demo_middle_ns'
}
}
stage ('nst-onboarding') {
steps {
sh 'osm nst-create slice_demo_nst/slice_demo_nst.yaml'    
}    
}
stage ('nst-instantiation') {
steps {
sh 'osm nsi-create --nsi_name slicedemo --nst_name slice_demo_nst --vim_account dummyvim'    
}
}
stage ('service validation') {
steps {
sh 'osm nst-list'
}
}
}
}
