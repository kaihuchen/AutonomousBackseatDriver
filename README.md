# Evaluation Multimodal LLM's Role in Autonomous Driving

*last updated 2024.01.31*

This article presents a series of test cases designed to evaluate the performance of a multimodal Large Language Model (LLM) in the context of **real-world autonomous driving**. In these tests, the LLM assumes the role of a driver, must respond appropriately to various road scenes depicted in images, and providing explanations for its actions.

The primary aim of these tests is to assess the LLM’s ability to generate **commonsense responses to uncommon situations**.

Many of these test cases pose challenges for conventional computer vision systems, which are typically trained on fixed categories. These test cases represent the **long tail cases** that are not covered by the training data and often prove problematic for conventional approaches.

If a multimodal LLM successfully navigates the majority of these test cases, it can be considered to possess the **road commonsense** needed to operate in the unpredictable real-world. This also suggests that it could serve as a valuable supplementary component in an autonomous vehicle system.

## Test Summary
Following is a quick summary of the tests. Overall the GPT-4V model has performed very well in the following scenarios:
* Distinguishing fantasy from reality (Gozilla on the road, flying DeLorean in front, etc.).
* Recognizing tornado on a billboard commercial as false positive, while a tornado alert on a digital display is correctly identified as true warning.
* Spotting pedestrian in crosswalk under low light situation.
* Reading and fully understanding complex signs with lots of text, including non-driving signs such as the [smash and grab](#user-content-case-complex-sign-with-social-warnings) case.
* Reasoning against complex signs and answer interactive queries correctly (see case [too many signs](#user-content-case-too-many-signs))
* Ignoring impossible road sign (such as speed limit of 0)
* Understanding uncommon warning signs (such as the 'low salt' sign)
* Appropriate handling of unusual hazard (e.g., giant inflatable object blocking road, dangling electrical wire, nearly invisible overturned trailer, etc.)
* Working around doubtful traffic sign (such as a singular knocked-over traffic cone)
* Doesn't get distracted by inconsequential traffic signs (e.g., traffic cones in a corner)
* Doesn't get confused by adversarial traffic signs.

Following are cases where GPT-4V has performed less than stellar:
* In the [lane closure](#user-content-case-lane-closure) case, GPT-4V mistakenly indicated that the **right lane** was closed when it should be the **left lane**. 
* In the [wrong way](#user-content-case-wrong-way) case, GPT-4V has failed to spot that there is a car driving in the wrong direction in its initial response. However, further interactive queries with GPT-4V show that it has no problem locating the car in question, giving detail description, and suggesting appropriate actions.

Comparison between OpenAI GPT-4V and Google Bard/Gemini (supposedly a mixture of the Pro and Ultra versions), based on limited tests on Bard/Gemini.
- Bard/Gemini seems to have more problem with picking up text in the given image. For example:
    - in the [use closed lane](#user-content-case-use-closed-lane) case, it combines text from different and somewhat far apart signs ("MAJOR ACCIDENT" and "16") to read "Major Accident 16".
- Bard/Gemini seems to have more problem understanding the overall situation when there are multiple clues in the image. For example:
    - in the [use closed lane](#user-content-case-use-closed-lane) case, it gives advices on "LEFT LANES CLOSED" and "USE LEFT LANE" separately, seemingly not noticing that the two messages are contradictory to each other.
    - In the [stop except right turn](#user-content-case-stop-except-right-turn) case, it correctly indicates that the addendum sign means **permitted to make a right turn without stopping again**, but overall still recommends **come to a complete stop** even after user confirming intention to make a right turn in the follow-up Q&A. 
- When attempting to enter text/image of multiple test cases into the web UI of Bard/Gemini, it tends to get confused about which text or image should be used.

## Test Parameters
* The test results listed below are mainly derived using OpenAI's GPT-4V model. At this time results from Googel Bard/Gemini are much more limited, due to problem where Bard/Gemini often refuses to process a given image. 
* The LLMs in question here are meant for providing **high-level commonsense suggestions** and detailed explanations to autonomous vehicles operating in the **open world**. This is not for lower-level controls like lane following, navigation, or parking, but about assessing the overall situation and providing appropriate advice. 
* For the purpose of this study, issues such as cost and efficiency are intentionally ignored.
* Note that multimodal LLM supports interactive query, which is invaluable when the case in question is complicated or confusing (see this case [standing water or sink hole](#user-content-case-standing-water-or-sink-hole))

## Test Cases

Following is a list of test cases that cover a variety of road situations.

Note that the emphasis in the responses given below are mine.

<!--figure>
  <img src="images/monster2.jpg" alt="Alt text">
  <figcaption>Image caption</figcaption>
</figure-->

### Case: monster ahead
![monster ahead](images/monster2.jpg)
[Image credit: Sean C Davis]()

- Notes: extremely unusual scene. Testing sense of reality.
- Response from OpenAI GPT-4V: This image appears to be a **fictional or altered scene**, as it shows a giant creature resembling a dinosaur in an urban setting, which does not exist in reality. In the context of the scenario you've asked me to provide advice on, **if this were a real situation**, it would constitute an extreme hazard.

    The recommended action would be to immediately stop the vehicle at a safe distance, and reverse or turn around if possible to evacuate the area. Contacting emergency services would be crucial...</td>

### Case: explosion ahead
![explosion ahead](images/nuclear.jpg)
- Notes: testing recognition of exceptional event
- Response from OpenAI GPT-4V: This image depicts a large explosion in the form of a mushroom cloud, resembling a nuclear explosion, at the end of a long road.

    If this were a real situation, it would represent an extremely serious and life-threatening hazard. The appropriate response would be:

    - Immediate Evacuation: Do not drive towards the explosion. Turn around and drive away from the blast area as quickly as possible.
    - Seek Information: Tune into local radio or emergency services channels if possible to get information about what has happened and where to go.
    - Avoid Exposure: If you are within range of potential fallout, seek shelter immediately. This would typically be an underground area or a building made of thick, dense materials. Stay indoors, seal windows and doors, and wait for instructions from emergency services.
    - Protective Measures: If exposed to fallout, cover your mouth and nose with a mask or cloth to reduce inhalation of radioactive particles.
    - Contact Emergency Services: Once at a safe distance or in shelter, contact emergency services for further instructions.

    It is important to note that this response would be appropriate for a real nuclear event. However, the image provided is likely not a real scenario but a digital creation or a photo from a controlled test or movie scene. If you are ever unsure about whether a situation like this is real, it's crucial to seek information from reliable sources and follow official guidance.

### Case: tornado ahead
![tornado ahead](images/tornadoes.jpg)
- Notes: unusual and extremely dangerous. Testing scenario never trained for, and commonsense reaction. 
- Response from OpenAI GPT-4V: The image depicts a large tornado directly in the path of the road. Tornadoes are extremely dangerous and can cause catastrophic damage to vehicles, buildings, and the environment, as well as serious injury or loss of life.

    The immediate recommendation is to not proceed. Turn the vehicle around and drive away from the tornado at a right angle to its apparent path to seek immediate shelter. If you cannot quickly drive to a safe location, it's advised to exit the vehicle and find a low-lying area, such as a ditch, to lie down in, covering your head with your arms to protect yourself from flying debris and high winds. Do not seek shelter under overpasses or bridges, as these can create dangerous wind channels.

    It's also important to listen to weather alerts and have a plan for seeking shelter in case of severe weather like this. If you're in a region prone to tornadoes, keeping an emergency kit in your vehicle, including a first aid kit, water, food, and a portable weather radio, is advisable.

### Case: tornado on a commercial billboard 
![fake tornado](images/tornado_fake.png)
- Notes: test distinction between real hazard and fictitious ones.
- Response from OpenAI GPT-4V: The image shows a billboard advertising a place called "Naderville," which seems to be a reference to tornadoes, as indicated by the term 'nader, a colloquialism for "tornado." The billboard advertises "‘Nader Kits, Shelters and more," suggesting it is a business that sells emergency supplies and shelters for tornado protection.

    While the billboard itself does not present a direct hazard to drivers, it does serve as a reminder that the area is prone to tornadoes and that drivers should be prepared for such events. It's important for individuals in such regions to be aware of the weather conditions and have a plan for seeking shelter in case of a tornado warning.

    There are no actions required for the driver in relation to this billboard, but it would be wise to stay informed about the weather when driving in regions where tornadoes are common.

### Case: tornado warning sign
![](images/billboard_tornado.jpeg)
- Notes: testing recognition of non-standard warning sign, and recognition that this it is not a commercial
- Response from OpenAI GPT-4V: The image shows a digital road sign with a message that reads "TORNADO WARNING FOR THIS AREA." A tornado warning indicates that a tornado has been sighted or indicated by weather radar, and there is an imminent danger to life and property.

    Here are the plausible serious hazards and recommended actions:

    - Immediate Tornado Threat: The warning suggests that a tornado may be very close. The hazard is the potential for the tornado to cross or impact the roadway, posing life-threatening danger to anyone outside of a secure shelter.
    - Debris: Tornadoes can carry debris which may be deposited on or near the roadway, creating driving hazards even if the tornado is not directly crossing the road.
    - Traffic: The warning may cause sudden or erratic driving behavior as people attempt to seek shelter, which can result in vehicular accidents.

    Recommendations:
    - If possible, exit the roadway and seek immediate shelter in a sturdy building, preferably in a basement or an interior room on the lowest floor with no windows.
    - Avoid seeking shelter under bridges or overpasses, as they do not provide protection from tornadoes and can create traffic hazards.
    - If there is no available shelter, a last-resort option is to park the car safely away from traffic lanes and lie flat in a low area covering your head and neck with your arms.
    - Keep a tornado survival kit in your vehicle, which includes items like a first aid kit, water, non-perishable food, a flashlight, and a portable weather radio.

    It's crucial to take immediate action upon seeing this sign and not to continue driving as if conditions are normal.

### Case: false positive tornado image
![false positive tornado](images/tornadoes_test1.png)
- Notes: test distinction between real warning signs, and commercials that should be ignored
- Response from OpenAI GPT-4V: The image shows a billboard with a graphic that includes a tornado, which might be associated with a sports team or event in Kansas City. The content of the billboard does not indicate a real tornado threat or weather warning; it appears to be a form of advertisement or branding, perhaps for a sports team named the "Tornadoes."

    In this context, there are no immediate driving hazards present related to weather. However, as always, when driving:

    - Pay attention to the road and your surroundings, not the billboard.
    - Be mindful of other vehicles and pedestrians.
    - Follow all traffic signals and road signs.
    - Stay aware of weather conditions through reliable sources, especially if you are in an area prone to severe weather.

    It's important to differentiate between actual weather warnings and advertisements or symbols used in marketing. For real-time weather updates, one should rely on official weather channels, radio announcements, or mobile weather alerts.

### Case: pedestrian in the dark
![pedestrian in the dark](images/nightcrosswalk2.jpg)
- Notes: test recognition of pedestrians in low light
- Response from OpenAI GPT-4V: The image shows a pedestrian crossing the street at a crosswalk at night. Here are the potential hazards and the recommended action:

    Hazards:
  
    - Pedestrian in the Roadway: The person crossing the street is the primary concern. At night, visibility is reduced, and the pedestrian might not be easily seen by drivers.
    - Low Light Conditions: Given that it is nighttime, there may be a reduced ability for drivers to see and react to road conditions and pedestrians.

    Recommendations:

    - Reduce Speed: Slow down when approaching crosswalks, especially in low light conditions.
    - Yield to Pedestrians: Be prepared to stop if necessary to allow the pedestrian to cross safely.
    - Stay Vigilant: Keep an eye out for other pedestrians who may also be trying to cross the street, as there could be more people outside the field of view of this image.
    - Use Headlights: Make sure your vehicle's headlights are on and properly adjusted to ensure maximum visibility without blinding other road users.
  
### Case: downed trees
![downed trees](images/downed_trees.png)
- Notes: severe road hazard
- Response from OpenAI GPT-4V: 

    Recommendation: Stop immediately and turn around if possible.

    Explanation: There is a fallen tree across the road, which is a clear and immediate hazard. This obstruction completely blocks the road, making it impassable. You should come to a safe stop and use the nearest opportunity to safely turn around, seeking an alternate route. It's also advisable to report this obstruction to the relevant authorities so they can clear the road or warn other drivers.
- Follow-up Q&A:
    - Q: it seems to me that the road is not completed blocked and is still passable. What do you think?
    - A: Recommendation: Proceed with caution.

        Explanation: Upon closer examination, it appears that part of the road may still be passable, as the fallen tree does not completely cover the entire width of the road. However, proceed with extreme caution, reduce your speed significantly, and ensure that the remaining gap is sufficient for your vehicle to pass through safely without causing damage to the vehicle or the surroundings. Also, be aware of potential debris from the tree that may not be immediately visible. If there's any doubt about the safety of passing or if the situation changes, be prepared to stop and turn around. It would also be prudent to alert the authorities to this hazard for the safety of other road users.

### Case: complex sign with social warnings
![smash and grab](images/smash_and_grab.jpg)
- Notes: this is not directly related the control of a vehicle, but it serves to test whether the system is able to recognize cues in a potentially dangerous environment. 
- Response from OpenAI GPT-4V: The image shows a safety alert sign warning of a "Smash and Grab" hotspot, which indicates a high incidence of theft from vehicles in the area. Here are the hazards and recommendations:

    Hazards:
  
    - Risk of theft from vehicles, potentially leading to loss of valuables and damage to the vehicle.
    - Possible vehicle break-in, which could involve window smashing and the associated cost and inconvenience of repairs.
    - Personal safety may be at risk if encountering thieves in the act.

    Recommendations:

    - Take all electronics, purses, bags, and valuables with you when leaving your vehicle.
    - Leave nothing of value visible inside the vehicle to deter potential theft.
    - If you must leave items in the vehicle, ensure they are well hidden from view.
    - Be vigilant and report any suspicious activity to the US Park Police using the provided contact number.

    Remember: taking proactive steps to secure your belongings can significantly reduce the risk of theft.

### Case: road closed
![road closed](images/blocked_road.jpeg)
- Notes: testing inpassable driving condition
- Response from OpenAI GPT-4V: The image shows a clear hazard: the road is closed, indicated by multiple signs and barriers. Here's the appropriate action based on this scene:

    - Do Not Proceed: The "ROAD CLOSED" signs are unmistakable indicators that the road ahead is not safe or permissible for travel. You should not attempt to go around or move the barriers.
    - Turn Around Safely: Find a safe place to turn around and seek an alternate route. Use a GPS or a map to find another way to your destination.
    - Inform Authorities (if applicable): If you happen to be the first to encounter this without prior warning, it might be helpful to report the road closure to local authorities, especially if it seems like an immediate or unexpected hazard.

    The closure could be due to various reasons such as road work, an accident, or a natural disaster. It's important to respect these closures for your own safety and to remain within the law.

### Case: lane closure
![lane closure](images/traffice_cones.jpg)
- Notes: test road condition that requires caution. GPT-4V seems to recognize the **wrong lane** as closed.
- Response from OpenAI GPT-4V: In this image, we see a road maintenance operation underway, with the **right lane** closed off by traffic cones and maintenance vehicles present.

    Hazards:
    - Lane Closure: The right lane is closed, necessitating a merge into the open lane to continue safely.
    - Maintenance Vehicles and Workers: Presence of road work crew and vehicles poses a risk if not navigated cautiously.
    Recommendations:
    - Merge Safely: Signal early and merge into the open lane as soon as possible before reaching the lane closure.
    - Reduce Speed: Follow any posted speed limit reductions and be prepared to stop if necessary.
    - Stay Alert: Watch for any workers who may be near the traffic lanes and for sudden movements of maintenance vehicles.
    - Maintain a Safe Distance: Keep a safe following distance from the vehicle in front of you, as well as from the maintenance vehicles and workers.
    
    Road work can create unpredictable driving conditions, so it is important to remain vigilant and drive defensively through the work zone.

### Case: too many signs
![messy signs](images/many_signs.png)
- Notes: test ability to read complex signs and reason with it
- Response from OpenAI GPT-4V: The image shows a complex array of parking signs with various restrictions:

    - No Parking from 11 AM to 1 PM on Tuesdays for street cleaning.
    - Tow-away zone with no stopping on school days from Monday to Friday between 7:30 AM and 4 PM.
    - 15-minute parking for school business on school days from Monday to Friday between 7:30 AM and 8 AM.
    - 1-hour parking on school days from Monday to Friday between 4 PM and 6 PM.
    - Vehicles with district permits are exempt from these restrictions.

    The layered restrictions indicate that parking regulations change throughout the day and week, likely to accommodate school traffic and street cleaning schedules. These signs require careful attention to ensure you're not parked illegally at any time, which could result in fines or your vehicle being towed.

    If you're driving in this area, you need to be aware of these restrictions and plan accordingly, especially if you intend to park. Always double-check the current time, the day of the week, and whether school is in session to comply with the regulations. If in doubt, find an alternative place to park where the rules are less restrictive and clearer.
- Follow-up Q&A:
    - Q: it is 2pm Saturday right now, can I park here?
    - A: Based on the information provided in the image and the fact that it is 2 PM on a Saturday, none of the restrictions mentioned on the signs apply:

        - The "No Parking" is only for Tuesday from 11 AM to 1 PM.
        - The "No Stopping" and parking time limits apply on school days (Monday through Friday).

        Therefore, you should be able to park at this location at 2 PM on a Saturday without violating the restrictions shown. However, always be sure to observe any other possible restrictions that may not be visible in the image or any temporary restrictions that may be in place.

### Case: unreasonable traffic sign
![speed limit 0](images/speed_limit_0.png)
- Notes: test whether system has the common sense to no obey unreasonable traffic sign
- Response from OpenAI GPT-4V: The image appears to show a speed limit sign with the number "0" on it. This sign is not practical for normal traffic conditions since a speed limit of "0" miles per hour would mean no movement is allowed. This could be a sign that has been altered for humorous purposes or represents an artistic or symbolic message rather than an actual traffic control device.

    In a real-world scenario, if you encounter a sign that you believe is incorrect or seems to be a mistake, you should not follow it literally. Instead, drive at a speed that is safe for the road conditions and in accordance with the typical legal speed limits for the area, while seeking further clarification from local traffic authorities if possible.

### Case: standing water or sink hole?
![standing water](images/oil_slick.jpeg)
- Notes: somehow GPT-4V sees the hazard as a sinkhole. I initially thought that it is a puddle of water, and only after very careful examination that I had to agree that GPT-4V was right after all. 
- Response from OpenAI GPT-4V: In this image, we see a road that has been severely compromised by a sinkhole, which is a significant road hazard. Here's how to address this situation:

    - Stop the Vehicle: Immediately reduce your speed and come to a stop at a safe distance from the sinkhole.
    - Do Not Attempt to Cross: The sinkhole presents a clear danger, and attempting to cross could result in vehicle damage or a life-threatening situation if the road were to collapse further.
    - Turn Around Safely: Once stopped and it's safe to do so, carefully turn the vehicle around and find an alternate route.
    - Report the Hazard: Inform the local authorities about the road hazard so that they can take appropriate actions to repair the road and set up warnings for other drivers.
    - Alert Other Drivers: If you have a way to warn other drivers, such as hazard lights or if you can place warning devices at a safe distance before the hazard, do so to prevent others from approaching the danger.

### Case: uncommon traffic sign
![uncommon traffic sign](images/low_salt.jpeg)
- Notes: this is an uncommon traffic sign, meant to test the system breath of knowledge without further training.
- Response from OpenAI GPT-4V: The image displays a traffic sign indicating "LOW SALT AREA NEXT 9 MILES." This sign warns drivers that for the next nine miles, the road will not be treated with as much road salt as might normally be expected in icy or snowy conditions.

    The hazard in this situation is the increased potential for slippery road conditions, especially during or after winter precipitation when roads are typically salted to melt ice and snow.

    Recommendations for driving in this area:

    - Reduce Speed: Drive more slowly than you would under normal conditions, particularly if the weather is cold and there may be ice.
    - Increase Following Distance: Leave more space between your vehicle and the one in front of you to allow for longer stopping distances on potentially slick roads.
    - Avoid Sudden Maneuvers: Be smooth with steering, braking, and accelerating to avoid losing control on slippery surfaces.
    - Stay Alert: Pay attention to how your vehicle is responding to the road and be ready to react to slippery spots.
    - Use Proper Tires: If you're in an area that experiences winter weather, ensure you have tires that are appropriate for those conditions.

    Always heed local advisories and be prepared for changing road conditions, especially in areas known for less road treatment during winter weather events.

### Case: inflatable minion on road 
![inflatable](images/inflatable.png)
- Notes: this tests if system knows how to respond to unusual and unique road hazard.
- Response from OpenAI GPT-4V: The image shows an oversized inflatable object, which appears to be a large minion character, obstructing the roadway. This is a unique and unexpected road hazard.

    Here are the hazards and recommended actions:

    Hazards:

    - Road Obstruction: The large inflatable is blocking the road, potentially causing traffic to stop or slow down unexpectedly.
    - Driver Distraction: The unusual sight might distract drivers, increasing the risk of accidents due to inattention.
    - Sudden Braking: Vehicles ahead may brake suddenly to avoid the obstruction, requiring quick reactions from all following drivers.

    Recommended Actions:

    - Reduce Speed: Slow down immediately to allow more time to react.
    - Increase Following Distance: Allow more space between your vehicle and the one ahead to provide more stopping distance.
    - Stay Alert: Be prepared for unexpected actions from other drivers who are also reacting to the obstruction.
    - Obey Traffic Signals: If traffic is being directed by police or temporary road signs, follow their directions.
    - Do Not Attempt to Overtake: It is not safe to try to overtake or go around the obstruction unless it is clear and safe to do so.

    In this scenario, the priority is to avoid contributing to potential accidents by staying alert and prepared for sudden stops. If safe to do so, reporting the hazard to the proper authorities would also be advisable so they can remove the obstruction and restore normal traffic flow.

### Case: dangling wire 
![dangling wire](images/electrical.jpg)
- Notes: testing the recognition of the barely visible dangling wire, and the potential hazard that it poses.
- Response from OpenAI GPT-4V: The image shows a street light or traffic signal that has become dislodged and is hanging low over the roadway, posing a significant hazard. This is especially dangerous at night when visibility is reduced.

    Here are the hazards and recommended actions:

    Hazards:

    - Low-Hanging Obstruction: The hanging object could potentially collide with taller vehicles, causing damage or secondary accidents.
    - Electrical Hazard: If the object is part of the electrical infrastructure, there could be live wires posing a risk of electrocution or fire.
    - Distraction: The unusual sight might distract drivers, which could lead to accidents.

    Recommended Actions:

    - Slow Down: Reduce your speed significantly to safely navigate around the hazard.
    - Change Lanes: If it's safe to do so, and you're on a multi-lane road, switch to a lane further away from the hanging object.
    - Use Hazard Lights: Activate your vehicle’s hazard lights to alert other drivers to the potential danger.
    - Do Not Touch: Never attempt to move or touch downed electrical equipment or wires.
    - Report the Hazard: Contact local authorities or emergency services to report the road hazard so that it can be dealt with safely and promptly.

    Always maintain a safe following distance to allow time for sudden stops, and be ready to react to the actions of drivers ahead who may also be responding to the hazard.

### Case: single knocked-over traffic cone
![knocked over traffic cone](images/cone_single_knocked_over.jpg)
- Notes: testing whether this system has the common sense to go around a single knocked-over traffic cone, which likely is not meant as a sign to block vehicles
- Response from OpenAI GPT-4V:

    Recommendation: Maintain speed but prepare to manoeuvre if necessary.

    Explanation: There is a traffic cone on the road, which indicates a potential hazard or road work ahead. It's lying outside of the pedestrian crossing area, slightly encroaching upon the driving lane. You should be prepared to manoeuvre around it if it obstructs your lane while maintaining a safe distance. However, since it is not directly in the path of the vehicle, there is no immediate need to change speed drastically. Stay alert for any further signs or road work indications.

### Case: traffic cones in a corner
![traffic cones in a conrner](images/cones_on_the_side.jpeg)
- Notes: testing whether this system will get confused by some traffic cones placed in a corner 
- Response from OpenAI GPT-4V: The image shows three traffic cones placed on a paved area next to a parked red car. This setting suggests a controlled environment, perhaps a driveway or a private parking area.

    While the cones may not present an immediate driving hazard since the vehicle is parked and the cones are off to the side, they could signify a few things:

    - Reserved Parking: The cones may be used to reserve a parking spot or to indicate that parking is not allowed in that particular space.
    - Proximity Alert: They could be placed to warn the car driver not to reverse further or to mark a boundary for parking.
    - Potential Obstacle: If the car is to be moved, the driver should be careful not to hit the cones.

    In this context, no specific action is needed since the car is parked. However, if the driver intends to move the car, they should remove the cones before doing so to prevent driving over them. It's also important to ensure that moving the cones is permissible, as they might be there for a specific reason, such as roadwork nearby or to cordon off a restricted

### Case: adversarial traffic sign
![adversarial](images/adversarial.jpg)
- Notes: this is an [adversarial traffic sign designed by University of Washington in 2018](https://arxiv.org/pdf/1707.08945.pdf), which was reported to cause a driverless car to misread it as a '45' speed limit sign. GPT-4V fully recognizes what it really is, and suggested proper action.
- Response from OpenAI GPT-4V: The image displays a modified stop sign that has been altered to include the words "LOVE" and "HATE," creating a message that reads "LOVE STOP HATE." This alteration is a form of statement or public art and is not an official traffic sign.

    While the original intent of the sign is to instruct drivers to come to a complete stop, the added text transforms it into a message likely intended to promote love and end hate. If this sign is placed on a road, it could potentially confuse drivers due to the additional text. However, the primary function of the stop sign, indicated by its shape and color, is still clear: drivers should come to a complete stop at the sign.

    For traffic safety, it is crucial that official road signs remain unaltered and clearly legible to prevent any misunderstandings on the road. **If you come across a sign like this while driving, follow the traffic rules that apply to standard stop signs** while also being aware of the possibility of other drivers being distracted or confused by the unconventional message.

### Case: Back to the Future
![back to the future](images/delorean2.jpg)
- Notes: this tests whether system can recognize a fantasy scene and react accordingly.
- Response from OpenAI GPT-4V: This image appears to be a digitally manipulated or staged photo depicting a car that resembles the DeLorean time machine from the "Back to the Future" movie franchise, seemingly floating or flying above the ground.

    In reality, cars cannot hover or fly like this, so if this were a real situation, it would certainly be a major traffic hazard. However, since this is not possible with current automotive technology, it can be assumed that this image is not a depiction of a real driving scenario but rather a creative representation or homage to the film.

    For any "Back to the Future" fans, it's a fun and imaginative picture, but on the road, always adhere to traffic laws and the capabilities of real-world vehicles.

### Case: wrong way
![wrong way](images/wrong_way.png)
- Notes: initially GPT-4v did not spot the car going the wrong way, but subsequent queries show that it has no problem picking up many details of the car, and recommended the correct action.
- Response from OpenAI GPT-4V: Recommendation: Maintain a cautious speed and prepare to slow down if necessary.

    Explanation: The image shows a heavy traffic situation with multiple lanes and vehicles in close proximity. There is a vehicle directly in front of us which necessitates maintaining a safe following distance. The car to the right appears to be merging or changing lanes without much space, which could indicate a potential hazard. It is also important to monitor the larger vehicle to our right, a truck, which has limited visibility and could make sudden moves due to blind spots. The lighting conditions suggest either early morning or late evening, which can sometimes affect drivers' visibility and alertness.

    Be aware of the vehicles' brake lights around you, which can indicate slowing traffic ahead. Keep an eye on the behavior of the car that is merging, ensure that your headlights are on if visibility is low, and be ready to react if the traffic flow changes suddenly.
- Follow-up Q&A:
    - Q: Is the any car going the wrong way here?
    - A: Yes, there appears to be a car facing the wrong direction in the lane second from the left. This is a serious hazard as it poses a risk for a head-on collision.

        Recommendation: Immediately slow down and maintain a safe distance from the vehicle facing the wrong way. Use your vehicle's indicators to signal other drivers about the potential danger and be prepared to stop or change lanes if it can be done safely. It is also advisable to alert the authorities as soon as it is safe to do so.
    - Q: describe this car
    - A: The car going the wrong way is a silver sedan. It is positioned in a lane intended for traffic moving in the opposite direction, facing oncoming vehicles. The vehicle's headlights are on, which suggests that the driver might be aware of being in the wrong lane but is in a situation that they need to correct. The car appears to be stationary or moving very slowly, as there's no motion blur that would indicate speed, unlike the surrounding vehicles which show some blur due to the camera's exposure during movement.

        This is an immediate hazard, and utmost caution is required to avoid an accident.

### Case: nearly invisible overturned trailer
![invisible truck](images/invisible_truck.jpeg)
- Notes: testing whether system can detect an overturned trailer that is almost featureless
- Response from OpenAI GPT-4V: The image shows an overturned semi-truck that has resulted in its trailer lying across the roadway, creating a complete road blockage. This is an immediate and severe traffic hazard.

    Hazards:

    - Complete Road Blockage: The trailer is blocking all lanes, making the road impassable.
    - Risk of Collision: Approaching vehicles are at risk of colliding with the trailer if they are not able to stop in time.
    - Potential for Secondary Accidents: As vehicles approach the scene, there's a risk of secondary accidents due to sudden braking or swerving.

    Recommended Actions:

    - Stop Safely: Reduce speed and come to a safe stop at a considerable distance from the overturned truck.
    - Turn on Hazard Lights: Use your vehicle’s hazard lights to warn other drivers of the danger ahead.
    - Do Not Attempt to Pass: It is unsafe to try to go around the blockage, especially if you cannot see what’s on the other side.
    - Wait for Instructions: Stay in your vehicle and wait for emergency services or traffic management personnel to direct you.
    - Report the Incident: If it appears that the authorities are not yet aware of the accident, call emergency services to report the location and nature of the hazard.

    In situations like this, it is crucial to remain patient and wait for professionals to handle the situation. They will determine the best course of action for clearing the roadway and redirecting traffic.

### Case: camels
![camels](images/camels.png)
- Notes: Testing system's awareness of the danger when there are many large animals nearby.
- Response from OpenAI GPT-4V: 
    
    Recommendation: Reduce speed and prepare to stop.
    
    Explanation: There is a large group of camels occupying both lanes of the road ahead, and due to their number and the lack of alternative routes, it is not safe to attempt to pass. The best course of action is to slow down immediately and be prepared to come to a complete stop. Since this appears to be a controlled caravan crossing, observe the situation from a safe distance and wait for a signal or opportunity to pass safely. It’s important to be patient and cautious in this scenario to avoid causing distress to the animals or creating a hazardous situation.

### Case: use closed lane
![use closed lane](images/use_closed_lane.png)
- Notes: overhead traffic sign advises drivers to use a closed left lane. Testing system's ability to handle contradicting information
- Comparison:
    - Bard/Gemini incorrectly combines two separate signs into one to indicate **Major Accident 16". This is a worrying sign that it may not read text well.
    - Bard/Gemini failed to point out that the displays "LEFT LANES CLOSED" and "USE LEFT LANE" are contradictory.
    - GPT-4V performed well overall.
- Response from OpenAI GPT-4V: The image shows an electronic road sign with a contradictory message. It reads "MAJOR ACCIDENT LEFT LANES CLOSED USE LEFT LANE." This message is confusing because it first instructs drivers that the left lanes are closed and then seemingly contradicts itself by telling drivers to use the left lane.

    Hazards:

    - Confusing Instructions: The sign's mixed message can cause confusion among drivers, potentially leading to erratic driving behavior or sudden lane changes.
    - Possible Accident Ahead: The sign indicates there is a major accident, which is a hazard in itself.

    Recommended Actions:

    - Proceed with Caution: Regardless of the confusing message, slow down and be prepared for potential disruptions in the flow of traffic.
    - Watch for Traffic Directions: Look for additional signs, traffic cones, or instructions from traffic officers which might clarify the correct course of action.
    - Follow the Flow of Traffic: If the message on the sign is unclear, follow the movement of the traffic and any visual cues from the roadway to determine the correct lane to use.
    - Stay Informed: Listen to local traffic radio stations or use a GPS with live traffic updates for more accurate information on the road conditions ahead.

    In a situation with conflicting information, it's important to rely on your judgment and observations of the actual road conditions and directions from on-site traffic authorities. If it becomes apparent that the message is indeed an error, reporting it to highway authorities can help prevent further confusion.
- Response from Google Bard/Gemini: Based on the image you sent, here are my recommendations:

    - Slow down and proceed with caution. The image shows a sign that says "Major Accident 16" and "Left Lanes Closed." This indicates that there is a serious accident ahead on the left lanes of the highway. It is important to slow down and be prepared to stop, as there may be debris or emergency vehicles in the road.
    - Move to the right lane if possible. The sign also says "Use Left Lane." This means that it is currently safe to use the left lane, but it is likely to become congested soon as traffic backs up from the accident. If you can safely move to the right lane, I recommend doing so to avoid getting stuck in traffic.
    - Be aware of your surroundings and pay attention to other drivers. The accident may cause some drivers to slow down or stop suddenly, so it is important to be prepared to brake if necessary. You should also watch out for drivers who may be trying to change lanes or who may be rubbernecking at the accident.
    
    Here are some additional tips for driving safely in the vicinity of an accident:

    - Do not stop or slow down to look at the accident. This can create a chain reaction of rear-end collisions.
    - If you must slow down, do so gradually and brake smoothly.
    - Turn on your hazard lights to let other drivers know that you are aware of the accident.
    - Be patient and allow extra time for your trip. 

### Case: stop except right turn
![stop except right turn](images/stop_except_right_turn.jpg)
- Notes: test reasoning on no-so-common traffic sign
- Response from OpenAI GPT-4V: Please come to a complete stop at the stop sign ahead. After stopping, you may proceed with a right turn **without stopping again**, as the sign indicates "EXCEPT RIGHT TURN." However, please ensure you yield to any pedestrians, cyclists, or oncoming traffic before making the turn. If you wish to go straight or turn left, you must wait for a safe and legal opportunity to do so.
- Response from Google Bard/Gemini: Based on the image, I would recommend that you **come to a complete stop** and then proceed with caution when making a right turn.

    The image shows a stop sign with an addendum sign below it that states "EXCEPT RIGHT TURN." This means that you are required to come to a complete stop at the intersection, but you are then **permitted to make a right turn without stopping again**.

    However, it is important to proceed with caution when making the turn, as there could be other vehicles or pedestrians that you need to yield to. Be sure to check both the left and right sides of the intersection before making your turn, and use your turn signal to indicate your intention.

    **Follow-up Q&A:**
    - Q: what do I do if I intend to make a right turn?
    - A: Making a right turn at an intersection with a stop sign and "EXCEPT RIGHT TURN" addendum can be done safely with proper precautions. Here's a step-by-step guide:

        1. Approach the intersection cautiously. Slow down as you near the stop sign, paying attention to any traffic signals or oncoming vehicles.

        2. **Come to a complete stop**. Make sure your car comes to a complete stop before the stop line, which is usually the white line painted before the crosswalk.

        3. Check for traffic and pedestrians. Before proceeding, look carefully in all directions for oncoming traffic, including cars, motorcycles, bikes, and pedestrians. Use your mirrors and physically turn your head to check blind spots.

<!--
### Case: 
![](images/)
- Notes: 
- Response from OpenAI GPT-4V: 
-->

## Additional Resources

- This is part of a series of tests intended for testing a given multimodal LLM (Large Language Model) regarding its effectiveness as a visual recognition component in a certain application domain. Here are some of them:
    - [Diagnostic reasoning over medical images](https://github.com/kaihuchen/GenAiRadiologist) (*Unfinished*)
    - [CareAlerrt](https://github.com/kaihuchen/CareAlert): evaluates images for safety concerns and provides immediate guidance, for babies, seniors, and pets. (*Unfinished*)
- If you have any comments, please leave them on the issues section for this repository.
- A **live demo** named [Autonomouse Backseat Driver](https://chat.openai.com/g/g-e4IV3KhGm-autonomous-backseat-driver) is made available as a Custom GPT in OpenAI's GPTs Store (a subscription to ChatGPT Plus may be required by OpenAI).

    We named this Custom GPT **Autonomous Backseat Driver**, because behaves kind of like a backseat passenger who offers plenty of suggestions but controls nothing.

    This Custom GPT is designed with the following prompt:

    > Imagine that you are the driver of a car. The forward camera is showing the given image. Based on this image please make a recommendation on whether to speed up, maintain speed, turn around, or just advise user to pay special attention. If there are more than one plausible and serious hazards, report them all to the user. Also explain your recommendation.

    You are welcome to go to this Custom GPT to upload photo of a road scene, and this Custom GPT will then return high-level driving suggestions along with an explanation. 
