# connectngo-vue-test

You are given a Vue.js component responsible for interacting with an API (e.g., a JIRA-like API). The component contains mistakes and room for improvement.

Your task is to:

- Identify mistakes and potential improvements in the Vue.js component.
- Refactor the code to address these issues.
- Provide explanations for the changes you made.

```js
<template>
    <div>
        <h1>JIRA Issues</h1>
        <button @click="getIssues">Fetch Issues</button>
        <button @click="getIssue('ABC-123')">Fetch Specific Issue</button>
        <button @click="createIssue">Create Issue</button>
        <button @click="updateIssue('ABC-123')">Update Issue</button>
        <button @click="deleteIssue('ABC-123')">Delete Issue</button>
        <div v-if="error">{{ error }}</div>
        <ul v-if="issues">
            <li v-for="issue in issues" :key="issue.id">{{ issue.title }}</li>
        </ul>
        <div v-if="specificIssue">
            <h2>{{ specificIssue.title }}</h2>
            <p>{{ specificIssue.description }}</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            issues: null,
            specificIssue: null,
            error: null,
            apiUrl: 'https://example-jira-api.com',
            username: 'myusername',
            apiToken: 'my-api-token'
        };
    },
    methods: {
        async getIssues() {
            try {
                const response = await fetch(`${this.apiUrl}/rest/api/2/search?jql=project=ABC`, {
                    headers: {
                        'Authorization': `Basic ${btoa(`${this.username}:${this.apiToken}`)}`
                    }
                });
                const data = await response.json();
                this.issues = data.issues;
            } catch (error) {
                this.error = 'Failed to fetch issues';
            }
        },
        async getIssue(issueId) {
            try {
                const response = await fetch(`${this.apiUrl}/rest/api/2/issue/${issueId}`, {
                    headers: {
                        'Authorization': `Basic ${btoa(`${this.username}:${this.apiToken}`)}`
                    }
                });
                const data = await response.json();
                this.specificIssue = data;
            } catch (error) {
                this.error = 'Failed to fetch specific issue';
            }
        },
        async createIssue() {
            const issueData = {
                fields: {
                    project: {
                        key: "ABC"
                    },
                    summary: "New Issue",
                    description: "Description for the new issue",
                    issuetype: {
                        name: "Bug"
                    }
                }
            };

            try {
                const response = await fetch(`${this.apiUrl}/rest/api/2/issue`, {
                    method: 'POST',
                    headers: {
                        'Authorization': `Basic ${btoa(`${this.username}:${this.apiToken}`)}`,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(issueData)
                });
                const data = await response.json();
                console.log('Issue created:', data);
            } catch (error) {
                this.error = 'Failed to create issue';
            }
        },
        async updateIssue(issueId) {
            const issueData = {
                fields: {
                    summary: "Updated Issue Summary",
                    description: "Updated issue description"
                }
            };

            try {
                const response = await fetch(`${this.apiUrl}/rest/api/2/issue/${issueId}`, {
                    method: 'PUT',
                    headers: {
                        'Authorization': `Basic ${btoa(`${this.username}:${this.apiToken}`)}`,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(issueData)
                });
                const data = await response.json();
                console.log('Issue updated:', data);
            } catch (error) {
                this.error = 'Failed to update issue';
            }
        },
        async deleteIssue(issueId) {
            try {
                const response = await fetch(`${this.apiUrl}/rest/api/2/issue/${issueId}`, {
                    method: 'DELETE',
                    headers: {
                        'Authorization': `Basic ${btoa(`${this.username}:${this.apiToken}`)}`
                    }
                });
                const data = await response.json();
                console.log('Issue deleted:', data);
            } catch (error) {
                this.error = 'Failed to delete issue';
            }
        }
    }
};
</script>


```
