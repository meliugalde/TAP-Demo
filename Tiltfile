SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='web-practice-fleet/tap-demo-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='webfleet')

k8s_custom_deploy(
    'tap-demo',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload tap-demo --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tap-demo', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'tap-demo'}])

allow_k8s_contexts('gke_web-practice-fleet_europe-west4-a_tap-reloaded')
            update_settings ( max_parallel_updates = 3 , k8s_upsert_timeout_secs = 180 , suppress_unused_image_warnings = None )

