# AutonomousBackseatDriver
This writeup explains how Generative AI can be used in an autonomous vehicle, as a visual commonsense component that offers high-level directives to the vehicle.

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
