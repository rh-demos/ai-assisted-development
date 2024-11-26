# Verify Code Intelligence

Modify the GreetingService code as User1 and submit it from gitlab page.

![image-20241030221404246](assets/5-verify-code-intelligence/image-20241030221404246.png)

The added function with issue is as follows:

```
    public void process() {
        String data = null;
        System.out.println(data.length()); 
    }
```

Create a merge request after the submit is completed, 

![image-20241030221549526](assets/5-verify-code-intelligence/image-20241030221549526.png)

Confirm the merge request.

![image-20241030221614981](assets/5-verify-code-intelligence/image-20241030221614981.png)

Navigates to the My Quarkus project with `root` user, you will find the Merge Request has been submitted and the pipeline has been triggered.

![image-20241030221710623](assets/5-verify-code-intelligence/image-20241030221710623.png)

Wait for the pipeline to complete, you will find the pipeline is failed.

![image-20241030221810676](assets/5-verify-code-intelligence/image-20241030221810676.png)

In the comment of the merge request, you will find that the sonarqube analysis report has been added.

![image-20241030221842686](assets/5-verify-code-intelligence/image-20241030221842686.png)

Continuing to scroll down, you can see that the code modification suggestions obtained by ai suggestion calling the llama model have been added to the comment of the merge request.

![image-20241126223913679](assets/5-verify-code-intelligence/image-20241126223913679.png)

from the ai suggestion log of OpenShift web console page, you will find that ai suggestion invoke the LLM and LLM  suggestions of code changes.

![image-20241030222205037](assets/5-verify-code-intelligence/image-20241030222205037.png)

Log in to gitlab as `user1`, comment out the code you just added, and commit

![image-20241030222235711](assets/5-verify-code-intelligence/image-20241030222235711.png)

You will find that the pipeline triggered by the merge request of the `My Quarkus` project under `dev` is completed and passed

![image-20241030222317119](assets/5-verify-code-intelligence/image-20241030222317119.png)

From the comment of the merge request, you will find that sonarqube analyzes the modification of the merge request and finds that there is a code smell. You can also see that the following comment is the modification suggestion of AI.

![image-20241126224245143](assets/5-verify-code-intelligence/image-20241126224245143.png)

if you want to fix the `Code Smell`, you can follow the AI suggstion to change the code.

Log in to gitlab review and you will find that the merge request CI generated earlier has been successful.

![image-20241030222458098](assets/5-verify-code-intelligence/image-20241030222458098.png)

Click the title part from gitlab reviewer, you will be navigated to the merge request page of gitlab, approve and merge.

![image-20241030222528575](assets/5-verify-code-intelligence/image-20241030222528575.png)

Return to gitlab review and you will find that there are no unfinished merge requests.

![image-20241030222631222](assets/5-verify-code-intelligence/image-20241030222631222.png)

After the code had been merged, the CI pipeline was triggered and built successfully.

![image-20241030222713352](assets/5-verify-code-intelligence/image-20241030222713352.png)

