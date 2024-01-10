# Autonomous Backseat Driver
Generative AI can play an important role in any autonomous vehicle that operates in the open world, 
as a visual component that offers high-level commonsense directives to the vehicle, with the ability to offer explanations to the human operator of the vehicle.

This write-up is about the following topics:
* A live demo named [Autonomouse Backseat Driver](https://chat.openai.com/g/g-UpQkvuX7j-artistic-transformer) is made available through OpenAI's GPTs Store.
There a user can upload photo of a road scene, and the GPT will then return high-level driving directions along with the reasons for the suggestion.
* The information below also serves as the guide for the users of the said demo.
* Additional information are provided to people interested in pursuing this topic,
regarding how to select edge-case road scenes to test out OpenAI's GPT-4V model.
We also welcome people to test the data on other multi-modal LLMs.

## Background
As of December 2023, both the Google Gemini and OpenAI GPT-4V are multimodal, meaning that they are capable of extracting meaningful textual information from a given image. When compared the typical CNN-based vision system, 

We named such a component an **Autonomous Backseat Driver**, because we expect this component to offer only higher-level advices to the vehicle, while leaving the low-level controls to other components. This is needed because autonoumous navigation in most cases must operate in an open environment with endless unexpected conditions, and a vision component that is trained narrowly on very specific categories won't have the commonsense to deal with complex situations.

Fortunately for us, the latest multimodal large language models have shown great promise in achieve zero-shot-learning visual recognition for road conditions, and often are able to offer good advice with commonsense, along with an explanation for the given advice.

## Live Demo
You can try for yourself with an **Autonomous Backseat Driver** that we implemented using an OpenAI Custom GPT

What you could do with this Custom GPT:

- You proceed to find a suitable photo that shows a road scene from a driver's view
- Go to our Custom GPT, and upload the photo
- The Custom GPT will then return with a decision on whether the vehicle should proceed normally, slow down, stop, or turn around. An explanation for the decision will also be given. 

## Examples

Following are some examples, collected here to demonstrate a number of points:

* There are endless situation on the open road that need to be handled well by any autonomous vehicle.
* There are many confusing situations that require some level of commonsense reasoning in order to reach good decision.
* Important decisions should be explained.

<table>
  <tr>
    <td>Image tested</td>
    <td>Notes</td>
    <td>Response from OpenAI GPT-4V</td>
    <td>Response from Google Gemini</td>
  </tr>
  <tr>
    <td>
      <img src="images/Billboard_stop_sign.png" style="width:300px" alt="Description of Image 1">
    </td>
    <td>
      This shows a fake stop sign on a billboard.\nCorrect reaction is to ignore it.
    </td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>
      <img src="images/tornadoes.jpg" style="width:300px" alt="Description of Image 2">
    </td>
    <td>
      Tornado ahead. Correct reaction is to turn around.
    </td>
    <td></td>
    <td></td>
  </tr>
</table>
