#!/usr/bin/env groovy

import hudson.model.*
import hudson.EnvVars
import org.jenkinsci.plugins.pipeline.modeldefinition.Utils
import org.apache.commons.io.FileUtils

timestamps
{
    node ()
    {
        def color = '#FB049E'
        def status = "SKIPPED"
        try
        {
            stage('Testing')
            {
                echo "--------------------------------------------------------+"
                echo BRANCH_NAME
                echo "--------------------------------------------------------+++"
                if (BRANCH_NAME.contains("feature") || BRANCH_NAME == "develop" || BRANCH_NAME == "qa" || BRANCH_NAME == "stage")
                {
                    echo "==Testing project....=="
                }
                else
                {
                    Utils.markStageSkippedForConditional(STAGE_NAME)
                }
            }

            stage('Develop')
            {
                if (BRANCH_NAME != "develop")
                {
                    Utils.markStageSkippedForConditional(STAGE_NAME)
                }
                else
                {
                    echo "Building ..."

                    status = "SUCCESS"
                }
            }

            stage('Stage')
            {
                if (BRANCH_NAME != "stage")
                {
                    Utils.markStageSkippedForConditional(STAGE_NAME)
                }
                else
                {
                    echo "Building ..."
                }
            }

            status = "SUCCESS"
            color = "#46D019"

        }
        catch ( e )
        {
            if (status != "SKIPPED")
            {
                status = "FAILLED"
            }
            currentBuild.result = 'FAILURE'
        }
        finally
        {
            if (status != "SKIPPED")
            {
                withCredentials([string(credentialsId: 'SLACK_EM_BE', variable: 'SLACK_TOKEN')])
                {
                    def slack_mes = "Build $status: $JOB_NAME #$BUILD_NUMBER \n(<$RUN_DISPLAY_URL|Open>)"
                    def slack_url = "https://itrex-kiev.slack.com/services/hooks/jenkins-ci/"
                    def slack_cha = "#emulatebio_builds"
                    //slackSend (baseUrl: "$slack_url", token: "${SLACK_TOKEN}", channel: "$slack_cha", color: color, message: slack_mes)
                }
            }

            //FileUtils.cleanDirectory(new File("$WORKSPACE"))
            FileUtils.deleteDirectory(new File("$WORKSPACE@tmp"))
            FileUtils.deleteDirectory(new File("$WORKSPACE@script"))
            FileUtils.deleteDirectory(new File("$WORKSPACE@script@tmp"))
        }
    }
}
