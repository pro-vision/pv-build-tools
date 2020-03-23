@Library('pipeline-library') pipelineLibrary
@Library('pv-pipeline-library') pvPipelineLibrary


import io.wcm.devops.jenkins.pipeline.ssh.SSHTarget

import static de.provision.devops.jenkins.pipeline.utils.ConfigConstants.*
import static io.wcm.devops.jenkins.pipeline.utils.ConfigConstants.*

// See:
// https://github.com/pro-vision/jenkins-pv-pipeline-library
// https://github.com/pro-vision/jenkins-pv-pipeline-library/blob/master/docs/config-structure.md
// Also have a look at https://github.com/wcm-io-devops/jenkins-pipeline-library for further configuration options

List triggers = defaults.getTriggers()
triggers.push(githubPush())

Map config = [
  (PROPERTIES)               : [
    (PROPERTIES_PIPELINE_TRIGGERS): triggers
  ],
  (STAGE_ANALYZE)            : [
    (MAVEN): [
      (MAVEN_GOALS): "checkstyle:checkstyle pmd:pmd",
    ],
  ],
  (STAGE_COMPILE)            : [
    (MAVEN): [
      (MAVEN_GOALS): ["clean", "deploy"],
    ]
  ],
  (STAGE_FEATURE_PREPARATION): [
    (STAGE_FEATURE_PREPARATION_MERGE): [
      (STAGE_FEATURE_PREPARATION_MERGE_ENABLED): false
    ]
  ]
]

routeDefaultJenkinsFile(config)
