# Firebase Backend Example
Launching a basic firebase backend for testing.
> The idea here is that I want to be able to version control within reason any resources that we are managing so that we can carefully track, manage and reproduce infrastructure changes.

## Project and services
These are the foundational resources for a Firebase project. These can easily be launched via Terraform. I have launched a carefully structured parent project and folder with granularly planned and implemented service accounts and roles. Anything above this parent project for the Firebase example are outside the scope of this example.
```hcl
# A Google Project base in a shared projects folder
resource "google_project" "parent" {
  project_id = var.project_id
  name = var.project_name
  folder_id = var.tf_managed_resources_folder_id
}

# A Firebase Project to enable Firebase on the base project
resource "google_firebase_project" "firebase" {
  provider = google-beta
  project = google_project.parent.project_id

  depends_on = [google_project_service.firebase]
}

# Required services enabled in Google Cloud
resource "google_project_service" "enabled_services" {
  for_each = toset([
    "appengine.googleapis.com",                          # App Engine Admin API
    "appenginereporting.googleapis.com",                 # App Engine
    "bigquery.googleapis.com",                           # BigQuery API
    "bigquerymigration.googleapis.com",                  # BigQuery Migration API
    "bigquerystorage.googleapis.com",                    # BigQuery Storage API
    "cloudapis.googleapis.com",                          # Google Cloud APIs
    "cloudresourcemanager.googleapis.com",               # Cloud Resource Manager API
    "cloudtrace.googleapis.com",                         # Cloud Trace API
    "fcmregistrations.googleapis.com",                   # FCM Registration API
    "firebase.googleapis.com",                           # Firebase Management API
    "firebaseappdistribution.googleapis.com",            # Firebase App Distribution API
    "firebasedatabase.googleapis.com",                   # Firebase Realtime Database Management API
    "firebasedynamiclinks.googleapis.com",               # Firebase Dynamic Links API
    "firebaseextensions.googleapis.com",                 # Firebase Extensions API
    "firebasehosting.googleapis.com",                    # Firebase Hosting API
    "firebaseinstallations.googleapis.com",              # Firebase Installations API
    "firebaseremoteconfig.googleapis.com",               # Firebase Remote Config API
    "firebaseremoteconfigrealtime.googleapis.com",       # Firebase Remote Config Realtime API
    "firebaserules.googleapis.com",                      # Firebase Rules API
    "identitytoolkit.googleapis.com",                    # Identity Toolkit API
    "logging.googleapis.com",                            # Cloud Logging API
    "mobilecrashreporting.googleapis.com",               # Mobile Crash Reporting API
    "monitoring.googleapis.com",                         # Cloud Monitoring API
    "oslogin.googleapis.com",                           # Cloud OS Login API
    "pubsub.googleapis.com",                            # Cloud Pub/Sub API
    "runtimeconfig.googleapis.com",                      # Cloud Runtime Configuration API
    "securetoken.googleapis.com",                        # Token Service API
    "storage-component.googleapis.com",                  # Cloud Storage
    "storage.googleapis.com",                           # Cloud Storage API
    "testing.googleapis.com",                           # Cloud Testing API
  ])
  
  project = var.project_id
  service = each.key
  disable_on_destroy = false
}
```