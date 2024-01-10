# AutonomousBackseatDriver
This writeup explains how Generative AI can be used in an autonomous vehicle, as a visual commonsense component that offers high-level directives to the vehicle.

We name such a component an **Autonomous Backseat Driver**, because we expect this component to offer only higher-level advices to the vehicle, while leaving the low-level controls to other components. This is needed because autonoumous navigation in most cases must operate in an open environment with endless unexpected conditions, and a vision component that is trained narrowly on very specific categories won't have the commonsense to deal with complex situations.

Fortunately for us, the latest multimodal large language models have shown great promise in achieve zero-shot-learning visual recognition for road conditions, and often are able to offer good advice with commonsense.



## Background
As of December 2023, both the Google Gemini and OpenAI GPT-4 are multimodal, meaning that they are capable of extracting meaningful textual information from a given image. When compared the typical CNN-based vision system, 

<table>
  <tr>
    <td>
      <img src="images/Billboard_stop_sign.png" style="width:300px" alt="Description of Image 1">
    </td>
    <td>
      This shows a fake stop sign on a billboard. Correct reaction is to ignore it.
    </td>
  </tr>
  <tr>
    <td>
      <img src="images/tornadoes.jpg" style="width:300px" alt="Description of Image 2">
    </td>
    <td>
      Tornado ahead. Correct reaction is to turn around.
    </td>
  </tr>
</table>
