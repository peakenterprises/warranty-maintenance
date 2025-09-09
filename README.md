<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home Issue Troubleshooting</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Basic styling to make it decent and functional */
        .container {
            max-width: 800px;
            margin: 20px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            font-family: sans-serif;
        }
        .flow-button {
            background-color: #4CAF50; /* Green */
            color: white;
            padding: 10px 20px;
            margin: 5px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color: 0.3s ease;
            width: 100%; /* Make buttons take full width of grid column */
        }
        .flow-button:hover {
            background-color: #45a049;
        }
        /* Style for the new Back button */
        .back-button {
            background-color: #6b7280; /* Gray */
            color: white;
            padding: 8px 16px;
            margin-bottom: 15px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            transition: background-color: 0.3s ease;
            width: auto; /* Allow button to size to content */
        }
        .back-button:hover {
            background-color: #4b5563;
        }
        .question-text {
            font-size: 1.25rem; /* Equivalent to text-xl in Tailwind */
            font-weight: 600; /* Equivalent to font-semibold in Tailwind */
            color: #333;
            margin-bottom: 1.5rem; /* Equivalent to mb-6 in Tailwind */
        }
        .decision-text {
            font-size: 1.1rem;
            color: #555;
            line-height: 1.6;
            white-space: pre-wrap; /* Preserve whitespace and line breaks */
        }
        .decision-text strong {
            color: #000; /* Make bold text stand out more */
        }
        .button-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* Responsive grid */
            gap: 10px;
        }
        .hidden {
            display: none;
        }
    </style>
</head>
<body class="bg-gray-100 p-4">
    <!-- =================== DISCLAIMER MODAL =================== -->
    <div id="disclaimer-overlay" class="fixed inset-0 bg-gray-900 bg-opacity-75 flex items-center justify-center p-4 z-50">
        <div class="bg-white rounded-lg shadow-xl p-6 md:p-8 max-w-2xl w-full">
            <h2 class="text-xl md:text-2xl font-bold text-gray-800 mb-4 text-center">IMPORTANT DISCLAIMER: PLEASE READ BEFORE PROCEEDING</h2>
            <div class="text-sm md:text-base text-gray-700 max-h-[60vh] overflow-y-auto pr-3 space-y-4">
                <p>
                    The following flowchart and information are provided by Peak Enterprises for informational and educational purposes ONLY. They are intended to help homeowners better understand potential issues and common pathways for resolution.
                </p>
                <p>
                    By proceeding, you acknowledge and agree to the following terms:
                </p>
                <ul class="list-disc list-inside space-y-2">
                    <li>
                        <strong>Not a Recommendation:</strong> This flowchart does not, in any way, constitute a recommendation, instruction, or professional advice from Peak Enterprises to perform any diagnosis, repair, or modification to your home.
                    </li>
                    <li>
                        <strong>No Responsibility for Actions:</strong> Peak Enterprises assumes NO RESPONSIBILITY or LIABILITY for any actions taken or work performed by a homeowner, contractor, or any third party based on the information presented. Any and all repairs or maintenance are undertaken at the homeowner's sole risk and discretion.
                    </li>
                    <li>
                        <strong>Consult a Professional:</strong> Home maintenance and repair can be complex and potentially hazardous. Peak Enterprises strongly advises all homeowners to consult with and hire a qualified, licensed, and insured professional to diagnose and address any issues with their home.
                    </li>
                    <li>
                        <strong>No Warranty:</strong> The information provided is general in nature and is supplied "as is" without any warranties, express or implied, of accuracy, completeness, or suitability for any specific situation.
                    </li>
                </ul>
                <p class="font-bold text-center">
                    You must not proceed if you do not agree to these terms.
                </p>
                <div class="mt-6 text-center">
                    <button id="accept-disclaimer-btn" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-3 px-8 rounded-lg transition-colors duration-300">
                        I Acknowledge and Agree
                    </button>
                </div>
            </div>
        </div>
    </div>
    <!-- =============================================================== -->
    <div class="container">
        <h1 class="text-2xl font-bold text-center mb-6">Home Issue Troubleshooting Assistant</h1>
        <div id="flowContent" class="mb-6">
            </div>
        <button id="resetButton" class="flow-button w-full bg-blue-500 hover:bg-blue-600 hidden">Start Over</button>
    </div>
    <script>
        // Define the flow tree structure
        const flowTree = {
            // ================================================================= //
            // == NEW STARTING NODE: Renter/Homeowner Check == //
            // ================================================================= //
            "user_type_check": {
                "question": "Are you a...",
                "options": [
                    { "text": "Homeowner", "next": "warranty_check" },
                    { "text": "Renter", "next": "renter_community_check" }
                ]
            },
            "renter_community_check": {
                "question": "What community do you live in?",
                "options": [
                    { "text": "Rancho Serenidad", "next": "renter_rancho_contact" },
                    { "text": "Villas De Mariposa", "next": "renter_villas_contact" },
                    { "text": "Brazos Point", "next": "renter_brazos_contact" },
                    { "text": "Solena", "next": "renter_solena_contact" }
                ]
            },
            "renter_rancho_contact": {
                "decision": {
                    "text": "Contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email."
                }
            },
            "renter_villas_contact": {
                "decision": {
                    "text": "Contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email."
                }
            },
            "renter_brazos_contact": {
                "decision": {
                    "text": "Contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email."
                }
            },
            "renter_solena_contact": {
                "decision": {
                    "text": "Contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email."
                }
            },
            // ================================================================= //
            // == EXISTING FLOW STARTS HERE == //
            // ================================================================= //
            "warranty_check": {
                "question": "Have you lived in your home for less than a year?",
                "options": [
                    { "text": "Yes", "next": "service_fee_warning" },
                    { "text": "No", "next": "warranty_expired_warning" }
                ]
            },
            "warranty_expired_warning": {
                "question": "Your warranty has expired and you as the homeowner are responsible for the maintenance to your home and any cost associated with it.",
                "options": [
                    { "text": "I Understand, Continue to Troubleshooter", "next": "start" }
                ]
            },
            "service_fee_warning": {
                "question": "Be advised, some warranties require a service fee that is not covered by the seller of the home. This service fee is due and payable by the homeowner to the specific service team or technician. Most, but not all, warranties have a $100-$200 service fee that is determined by the warranty company. You should ask your warranty company if a service fee is required for their service that you, the homeowner, will be responsible for. Weekend, holiday, or after hours calls may be slightly higher in cost.",
                "options": [
                    { "text": "Acknowledge and Continue", "next": "start" }
                ]
            },
            "start": {
                "question": "Topics of interest.",
                "options": [
                    { "text": "Electrical", "next": "electrical_start" },
                    { "text": "Structural", "next": "structural_start" },
                    { "text": "Water", "next": "water_start_new" },
                    { "text": "Interior", "next": "interior_start" },
                    { "text": "Exterior", "next": "exterior_start" },
                    { "text": "Appliance/HVAC", "next": "appliance_hvac_start" },
                    { "text": "Yard Related", "next": "yard_community_check" },
                    { "text": "Community Related", "next": "community_community_check" },
                    { "text": "Preferred Vendors", "next": "vendors_placeholder" },
                    { "text": "Warranty Contact Numbers", "next": "contacts_placeholder" },
                    { "text": "Issue not listed", "next": "not_listed_placeholder" }
                ]
            },
            // The rest of the original flowTree object...
            // Electrical Path
            "electrical_start": { "question": "What is the electrical issue?", "options": [ { "text": "Outlet not working", "next": "electrical_outlet_ack" }, { "text": "Light fixture not working", "next": "electrical_light_fixture_ack" }, { "text": "Ceiling fan not working", "next": "electrical_ceiling_fan_ack" }, { "text": "Breaker keeps tripping", "next": "electrical_breaker_tripping_ack" }, { "text": "My home has no electricity.", "next": "electrical_no_electricity_ack" } ] },
            "electrical_no_electricity_ack": {
                "question": "You may be asked if you have a flipped breaker in the following questions. A flipped breaker in the main breaker panel will feel spongy when pressed and needs to be switched to off and back to on to reset the breaker. If you are asked to reset a GFCI, this is the outlet with a button in the middle of it. You may need to press this button to reset the outlet",
                "options": [{ "text": "Acknowledge and Continue", "next": "electrical_home_no_electricity" }]
            },
            "electrical_light_fixture_ack": {
                "question": "You may be asked if you have a flipped breaker in the following questions. A flipped breaker in the main breaker panel will feel spongy when pressed and needs to be switched to off and back to on to reset the breaker. If you are asked to reset a GFCI, this is the outlet with a button in the middle of it. You may need to press this button to reset the outlet",
                "options": [{ "text": "Acknowledge and Continue", "next": "electrical_light_fixture_issue" }]
            },
            "electrical_ceiling_fan_ack": {
                "question": "You may be asked if you have a flipped breaker in the following questions. A flipped breaker in the main breaker panel will feel spongy when pressed and needs to be switched to off and back to on to reset the breaker. If you are asked to reset a GFCI, this is the outlet with a button in the middle of it. You may need to press this button to reset the outlet",
                "options": [{ "text": "Acknowledge and Continue", "next": "electrical_ceiling_fan_issue" }]
            },
            "electrical_breaker_tripping_ack": {
                "question": "You may be asked if you have a flipped breaker in the following questions. A flipped breaker in the main breaker panel will feel spongy when pressed and needs to be switched to off and back to on to reset the breaker. If you are asked to reset a GFCI, this is the outlet with a button in the middle of it. You may need to press this button to reset the outlet",
                "options": [{ "text": "Acknowledge and Continue", "next": "electrical_breaker_tripping" }]
            },
            "electrical_outlet_ack": {
                "question": "You may be asked if you have a flipped breaker in the following questions. A flipped breaker in the main breaker panel will feel spongy when pressed and needs to be switched to off and back to on to reset the breaker. If you are asked to reset a GFCI, this is the outlet with a button in the middle of it. You may need to press this button to reset the outlet",
                "options": [{ "text": "Acknowledge and Continue", "next": "electrical_outlet_issue" }]
            },
            "electrical_home_no_electricity": {
                "question": "Have you checked the main breaker panel for a tripped breaker? A tripped breaker will feel spongy when you press on it. Flip it to off and then back on. If it is a GFCI outlet, have you tried to reset the outlet itself?",
                "options": [
                    { "text": "This fixed the issue.", "next": "electrical_issue_fixed" },
                    { "text": "This did not fix the issue.", "next": "electrical_issue_not_fixed" }
                ]
            },
            "electrical_issue_fixed": { "decision": { "text": "Great! Glad we could help get your power back on. If it happens again, feel free to come back to this section." } },
            "electrical_issue_not_fixed": { "decision": { "text": "If the main breaker is not tripped, there could be an issue with your meter, main line from the electrical pole, or the main breaker itself. Contact a licensed electrician or your warranty department at **warranty@peakmhc.com** for an assessment." } },
            "electrical_outlet_issue": { "question": "Have you checked the breaker in the main breaker panel for a tripped breaker? A tripped breaker will feel spongy when you press on it. Flip it to off and then back on. If it is a GFCI outlet, have you tried to reset the outlet itself?", "options": [ { "text": "Yes, the breaker has flipped or the GFCI tripped.", "next": "electrical_outlet_breaker_tripped" }, { "text": "No, the breaker is on and the GFCI is not tripped.", "next": "electrical_outlet_no_breaker_issue" } ] },
            "electrical_outlet_breaker_tripped": { "decision": { "text": "Reset the breaker or GFCI. If it immediately trips again, there's likely an electrical short or overload. Contact your warranty department at warranty@peakmhc.com or a licensed electrician immediately.", "action": "Reset breaker/GFCI; if trips/fails, contact warranty/electrician." } },
            "electrical_outlet_no_breaker_issue": { "decision": { "text": "If the breaker is on and the GFCI is not tripped, the outlet itself may be faulty or improperly wired. Contact your warranty department at warranty@peakmhc.com or a licensed electrician for diagnosis and repair.", "action": "Contact warranty/electrician." } },
            "electrical_light_fixture_issue": { "question": "Have you replaced the light bulb?", "options": [ { "text": "Yes, I replaced the bulb and it still doesn't work.", "next": "electrical_light_fixture_bulb_replaced" }, { "text": "No, I haven't replaced the bulb yet.", "next": "electrical_light_fixture_replace_bulb" } ] },
            "electrical_light_fixture_replace_bulb": { "decision": { "text": "Replace the light bulb. If the issue persists, return to the previous step.", "action": "Replace light bulb." } },
            "electrical_light_fixture_bulb_replaced": { "question": "Have you checked the breaker in the main breaker panel?", "options": [ { "text": "The breaker has flipped", "next": "electrical_light_fixture_breaker_tripped" }, { "text": "The breaker has not flipped", "next": "electrical_light_fixture_breaker_not_flipped" } ] },
            "electrical_light_fixture_breaker_tripped": { "decision": { "text": "Reset the breaker. If it immediately trips again, there's likely an electrical short. Contact your warranty department at warranty@peakmhc.com or a licensed electrician immediately.", "action": "Reset breaker; if trips/fails, contact warranty/electrician." } },
            "electrical_light_fixture_breaker_not_flipped": { "decision": { "text": "If the breaker is on, the issue is likely within the light fixture itself, such as a faulty socket or wiring. Contact your warranty department at warranty@peakmhc.com  or a licensed electrician for diagnosis and repair.", "action": "Contact warranty/electrician." } },
            "electrical_breaker_tripping": { "question": "Is it a specific appliance or device that causes the breaker to trip, or does it seem random?", "options": [ { "text": "A specific appliance/device causes it to trip.", "next": "electrical_breaker_appliance_cause" }, { "text": "It seems to trip randomly or without a clear cause.", "next": "electrical_breaker_random_trip" } ] },
            "electrical_breaker_appliance_cause": { "decision": { "text": "Unplug the appliance that causes the trip. If the breaker holds, the appliance is likely faulty. If multiple appliances on the same circuit cause it, you may be overloading the circuit. If the breaker still trips without the appliance, or if it's a new issue, contact your **warranty department at warranty@peakmhc.com** or a **licensed electrician**.", "action": "Unplug faulty appliance/reduce load; contact warranty/electrician if issue persists." } },
            "electrical_breaker_random_trip": { "decision": { "text": "Random breaker trips can indicate a serious underlying electrical issue, such as a short in the wiring or a faulty breaker. This requires immediate attention. Contact your warranty department at warranty@peakmhc.com or a licensed electrician immediately.", "action": "Contact warranty/electrician urgently." } },
            "electrical_ceiling_fan_issue": { "question": "What is the issue with your ceiling fan?", "options": [ { "text": "Fan hums but doesn't spin or spins slowly.", "next": "electrical_fan_hums" }, { "text": "Fan doesn't turn on at all (no light, no fan movement).", "next": "electrical_fan_no_power" }, { "text": "Pull chain/string doesn't do anything.", "next": "electrical_fan_pull_string_no_response" }, { "text": "Fan wobbles excessively or makes grinding/clicking noises.", "next": "electrical_fan_wobble_noise" } ] },
            "electrical_fan_hums": { "decision": { "text": "This could indicate a faulty capacitor or a motor issue. Ensure the fan blades are unobstructed and balanced. If the issue persists, contact your warranty department at warranty@peakmhc.com or a licensed electrician.", "action": "Check for obstructions/balance; contact warranty/electrician." } },
            "electrical_fan_no_power": { "question": "Have you checked the breaker in the main breaker panel?", "options": [ { "text": "The breaker has flipped", "next": "electrical_fan_breaker_flipped" }, { "text": "The breaker has not flipped", "next": "electrical_fan_breaker_not_flipped" } ] },
            "electrical_fan_pull_string_no_response": { "question": "Have you checked the breaker in the main breaker panel?", "options": [ { "text": "The breaker has flipped", "next": "electrical_fan_breaker_flipped" }, { "text": "The breaker has not flipped", "next": "electrical_fan_breaker_not_flipped" } ] },
            "electrical_fan_wobble_noise": { "decision": { "text": "Excessive wobble usually means loose mounting or unbalanced blades. Ensure all screws are tight and use a balancing kit. Grinding/clicking suggests motor or bearing issues. If balancing doesn't help or noises persist, contact your **warranty department** or a **licensed electrician**.", "action": "Check mounting/balance blades; contact warranty/electrician if persistent." } },
            "electrical_fan_breaker_flipped": { "decision": { "text": "Reset the breaker. If it immediately trips again, there's likely an electrical short. Contact your **warranty department at warranty@peakmhc.com** or a **licensed electrician** immediately. If it stays on but the fan still doesn't work, proceed to contact for repair.", "action": "Reset breaker; if trips/fails, contact warranty/electrician." } },
            "electrical_fan_breaker_not_flipped": { "decision": { "text": "If the breaker is on, the issue is likely within the fan unit itself, such as a faulty motor, switch, or wiring. Contact your **warranty department at warranty@peakmhc.com** or a **licensed electrician** for diagnosis and repair.", "action": "Contact warranty/electrician." } },
            "water_start_new": { "question": "Is the water issue inside or outside the home?", "options": [ { "text": "The issue is inside", "next": "water_issue_inside" }, { "text": "The issue is outside", "next": "water_issue_outside" } ] },
            "water_issue_inside": { "question": "What is the issue inside the home?", "options": [ { "text": "The issue is with a toilet", "next": "water_toilet_start_new" }, { "text": "The issue is with a tub/shower", "next": "water_tub_shower_start" }, { "text": "The issue is with a sink", "next": "water_sink_start" }, { "text": "The issue is in the laundry area", "next": "water_laundry_start" }, { "text": "There is water coming from the AC/water heater closet", "next": "water_ac_heater_closet_start" }, { "text": "The dishwasher is leaking", "next": "water_dishwasher_leaking" }, { "text": "There is a leak in my ceiling after it rains", "next": "water_ceiling_leak_after_rain" } ] },
            "water_toilet_start_new": { "question": "What is the issue with your toilet?", "options": [ { "text": "The water in my toilet continues to run.", "next": "water_toilet_running_new" }, { "text": "The toilet will not flush, or it is overflowing", "next": "water_toilet_clog_overflow" } ] },
            "water_toilet_running_new": { "decision": { "text": "This is a **homeowner’s responsibility**. You may need to adjust the fill level, repair the flush mechanism, or replace a seal. You can perform this yourself or contact a service technician. This repair and the expense of it, is the responsibility of the homeowner. Not repairing this will cause an increase to your water bill.", "action": "Homeowner responsibility; adjust/repair/replace components or contact technician." } },
            "water_toilet_clog_overflow": { "question": "Did you flush something by accident?", "options": [ { "text": "Yes", "next": "water_toilet_clog_flushed_item" }, { "text": "No", "next": "water_toilet_clog_no_item" } ] },
            "water_toilet_clog_flushed_item": { "decision": { "text": "You will need to plunge the toilet with a plunger to clear the clog. If this does not work you will need to contact a **certified technician**. This is a homeowner's responsibility and the homeowner will be responsible for any expenses incurred.", "action": "Plunge toilet; if persistent, contact technician (homeowner expense)." } },
            "water_toilet_clog_no_item": { "decision": { "text": "If you have tried plunging the toilet and the toilet still does not flush contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice. It will be reviewed to determine responsibility.", "action": "Plunge toilet; if persistent, contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_tub_shower_start": { "question": "What is the issue with your tub/shower?", "options": [ { "text": "The tub/shower will not drain", "next": "water_tub_shower_not_drain" }, { "text": "There is water leaking from behind the fixture in the tub/shower", "next": "water_tub_shower_fixture_leak" }, { "text": "There is water leaking into the floor when the tub/shower is in use or is draining", "next": "water_tub_shower_floor_leak" } ] },
            "water_tub_shower_not_drain": { "question": "Is there something in the drain?", "options": [ { "text": "Yes", "next": "water_tub_shower_drain_debris" }, { "text": "No", "next": "water_tub_shower_drain_no_debris" } ] },
            "water_tub_shower_drain_debris": { "decision": { "text": "Clear the debris from the drain and see if this helps.", "action": "Clear debris from drain." } },
            "water_tub_shower_drain_no_debris": { "decision": { "text": "If you have tried plunging the toilet and the toilet still does not flush contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice. It will be reviewed to determine responsibility.", "action": "Plunge toilet; if persistent, contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_tub_shower_fixture_leak": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_tub_shower_fixture_leak_after_hours" }, { "text": "No", "next": "water_tub_shower_fixture_leak_during_hours" } ] },
            "water_tub_shower_fixture_leak_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_tub_shower_fixture_leak_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_tub_shower_floor_leak": { "question": "Where is the water coming from?", "options": [ { "text": "The water is coming from under the tub/shower", "next": "water_tub_shower_under_leak" }, { "text": "There is water leaking from the wall in the room behind the tub/shower", "next": "water_tub_shower_wall_leak_behind" } ] },
            "water_tub_shower_under_leak": { "decision": { "text": "Discontinue use of the tub/shower and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Discontinue use; email warranty@peakmhc.com with details and photos." } },
            "water_tub_shower_wall_leak_behind": { "decision": { "text": "Discontinue use of the tub/shower and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Discontinue use; email warranty@peakmhc.com with details and photos." } },
            "water_sink_start": { "question": "What is the issue with your sink?", "options": [ { "text": "When I turn the water on it leaks from the fixture", "next": "water_sink_fixture_leak" }, { "text": "There is a water leak under the sink", "next": "water_sink_under_leak_start" } ] },
            "water_sink_fixture_leak": { "decision": { "text": "Discontinue use of the sink and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Discontinue use; email warranty@peakmhc.com with details and photos." } },
            "water_sink_under_leak_start": { "question": "Where is the water leaking from under the sink?", "options": [ { "text": "The water coming from the P-Trap. The P-Trap is a white pipe that loops down and back up. Sometimes this becomes loose and can leak.", "next": "water_sink_leak_ptrap" }, { "text": "The water is leaking from one of the water lines.", "next": "water_sink_leak_water_line_start" }, { "text": "The valve is leaking", "next": "water_sink_leak_valve_start" } ] },
            "water_sink_leak_ptrap": { "decision": { "text": "Check the P-Trap and ensure the fitting is tight. The P-Trap is a white pipe that loops down and back up. Sometimes this becomes loose and can leak. This is something that as a homeowner should be checked regularly. Any damage caused by this leak will not be considered warranty and the homeowner is responsible for any repairs caused by neglecting to check this regularly.", "action": "Check/tighten P-Trap; homeowner responsibility for associated damage." } },
            "water_sink_water_line_leak_start": { "question": "Is the leak after or before the valve?", "options": [ { "text": "After", "next": "water_sink_water_line_leak_after_valve" }, { "text": "Before", "next": "water_sink_water_line_leak_before_valve_time_check" } ] },
            "water_sink_water_line_leak_after_valve": { "decision": { "text": "Turn the water off at the valve. Discontinue use of the sink and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off valve; discontinue use; email warranty@peakmhc.com with details and photos." } },
            "water_sink_water_line_leak_before_valve_time_check": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_sink_water_line_leak_before_valve_after_hours" }, { "text": "No", "next": "water_sink_water_line_leak_before_valve_during_hours" } ] },
            "water_sink_water_line_leak_before_valve_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_sink_water_line_leak_before_valve_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater\", and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_sink_leak_valve_start": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_sink_leak_valve_after_hours" }, { "text": "No", "next": "water_sink_leak_valve_during_hours" } ] },
            "water_sink_leak_valve_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_sink_leak_valve_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_laundry_start": { "question": "What is the issue in the laundry area?", "options": [ { "text": "There is water leaking from my washing machine", "next": "water_laundry_washing_machine_leak" }, { "text": "There is water leaking from where the water hose of the washing machine hooks up", "next": "water_laundry_hose_hookup_leak" }, { "text": "There is water leaking from under the wall", "next": "water_laundry_under_wall_leak" } ] },
            "water_laundry_washing_machine_leak": { "decision": { "text": "This is a **homeowner’s responsibility** and not a warranty issue. You will need to contact a service technician at your own expense to repair the issue.", "action": "Homeowner responsibility; contact service technician at own expense." } },
            "water_laundry_hose_hookup_leak": { "question": "Is it on the washing machine side of the valve?", "options": [ { "text": "Yes", "next": "water_laundry_hose_leak_machine_side" }, { "text": "It is leaking at the valve", "next": "water_laundry_hose_leak_valve_time_check" } ] },
            "water_laundry_hose_leak_machine_side": { "decision": { "text": "This is a **homeowner’s responsibility** and not a warranty issue. You will need to contact a service technician at your own expense to repair the issue.", "action": "Homeowner responsibility; contact service technician at own expense." } },
            "water_laundry_hose_leak_valve_time_check": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_laundry_hose_leak_valve_after_hours" }, { "text": "No", "next": "water_laundry_hose_leak_valve_during_hours" } ] },
            "water_laundry_hose_leak_valve_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_laundry_hose_leak_valve_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_laundry_under_wall_leak": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_laundry_under_wall_leak_after_hours" }, { "text": "No", "next": "water_laundry_under_wall_leak_during_hours" } ] },
            "water_laundry_under_wall_leak_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_laundry_under_wall_leak_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_ac_heater_closet_start": { "question": "What is the issue in the AC/water heater closet?", "options": [ { "text": "There is water spraying from the water heater", "next": "water_ac_heater_closet_heater_spray" }, { "text": "There is water coming from the air handler in the closet", "next": "water_ac_heater_closet_air_handler_start" } ] },
            "water_ac_heater_closet_heater_spray": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_ac_heater_closet_heater_spray_after_hours" }, { "text": "No", "next": "water_ac_heater_closet_heater_spray_during_hours" } ] },
            "water_ac_heater_closet_heater_spray_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_ac_heater_closet_heater_spray_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_ac_heater_closet_air_handler_start": { "question": "Remove the front of the air handler to expose the coils. If coils are covered with ice, turn off the system and contact your HVAC warranty company. What park do you live in?", "options": [ { "text": "Rancho Serenidad MHC", "next": "water_ac_heater_closet_air_handler_rancho" }, { "text": "Villas De Mariposa MHC", "next": "water_ac_heater_closet_air_handler_villas" }, { "text": "Solena MHC", "next": "water_ac_heater_closet_air_handler_solena" }, { "text": "Brazos Point MHC", "next": "water_ac_heater_closet_air_handler_brazos" } ] },
            "water_ac_heater_closet_air_handler_rancho": { "decision": { "text": "Contact Champion Heating and Air at **installservice@championheatingair.net** or **682-230-5566**. You will need your serial number of your unit. The information is located on your data sheet under your kitchen sink.", "action": "Contact Champion Heating and Air with unit serial number." } },
            "water_ac_heater_closet_air_handler_villas": { "decision": { "text": "Contact Champion Heating and Air at **installservice@championheatingair.net** or **682-230-5566**. You will need your serial number of your unit. The information is located on your data sheet under your kitchen sink.", "action": "Contact Champion Heating and Air with unit serial number." } },
            "water_ac_heater_closet_air_handler_solena": { "decision": { "text": "Contact Champion Heating and Air at **installservice@championheatingair.net** or **682-230-5566**. You will need your serial number of your unit. The information is located on your data sheet under your kitchen sink.", "action": "Contact Champion Heating and Air with unit serial number." } },
            "water_ac_heater_closet_air_handler_brazos": { "decision": { "text": "Contact Mr. Chill at **936-236-9241**. You will need your serial number of your unit. The information is located on your data sheet under your kitchen sink.", "action": "Contact Mr. Chill with unit serial number." } },
            "water_dishwasher_leaking": { "question": "How is the dishwasher leaking?", "options": [ { "text": "It leaks when it is draining after running a cycle", "next": "water_dishwasher_drain_leak" }, { "text": "It leaks constantly even when it is not in use", "next": "water_dishwasher_constant_leak_time_check" } ] },
            "water_dishwasher_drain_leak": { "question": "Have you checked the hose on the dishwasher and where it connects to the drain line under the kitchen sink to ensure it is tight and connected well?", "options": [ { "text": "This fixed the issue", "next": "water_dishwasher_drain_leak_fixed" }, { "text": "These connections are tight, but it still leaks", "next": "water_dishwasher_drain_leak_persistent" } ] },
            "water_dishwasher_drain_leak_fixed": { "decision": { "text": "Great! Ensuring drain connections are tight often resolves this issue. Monitor to ensure the leak does not reoccur.", "action": "Issue resolved; monitor." } },
            "water_dishwasher_drain_leak_persistent": { "decision": { "text": "Discontinue use of the dishwasher and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Discontinue use; email warranty@peakmhc.com with details and photos." } },
            "water_dishwasher_constant_leak_time_check": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_dishwasher_constant_leak_after_hours" }, { "text": "No", "next": "water_dishwasher_constant_leak_during_hours" } ] },
            "water_dishwasher_constant_leak_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_dishwasher_constant_leak_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_ceiling_leak_after_rain": { "question": "Have you inspected your roof?", "options": [ { "text": "There is no noticeable damage", "next": "water_ceiling_leak_no_roof_damage" }, { "text": "There is damage to the roof in the area of concern", "next": "water_ceiling_leak_roof_damage" } ] },
            "water_ceiling_leak_no_roof_damage": { "decision": { "text": "Contact a **certified roofer** and have them inspect your roof. Your roof warranty covers material failure and workmanship of the installation of it. It will not cover weather-related issues, and you will be responsible for any expenses incurred. Depending on the amount of the repair you may choose to involve your homeowner’s insurance policy. If the roofing company determines it is a warranty issue they will need to document the issue well with before, during, and after photos of the repair. Submit the invoice and their report, with the photos, to **warranty@peakmhc.com** for review to determine responsibility.", "action": "Contact certified roofer; if warranty issue, document and submit to warranty@peakmhc.com." } },
            "water_ceiling_leak_roof_damage": { "decision": { "text": "Contact a **certified roofer** and have them inspect your roof. Your roof warranty covers material failure and workmanship of the installation of it. It will not cover weather-related issues, and you will be responsible for any expenses incurred. Depending on the amount of the repair you may choose to involve your homeowner’s insurance policy. If the roofing company determines it is a warranty issue they will need to document the issue well with before, during, and after photos of the repair. Submit the invoice and their report, with the photos, to **warranty@peakmhc.com** for review to determine responsibility.", "action": "Contact certified roofer; if warranty issue, document and submit to warranty@peakmhc.com." } },
            "water_issue_outside": { "question": "What is the issue outside the home?", "options": [ { "text": "There is water coming from under my home", "next": "water_outside_leak_under_home_temp_check" }, { "text": "I smell sewer", "next": "water_outside_sewer_smell_start" } ] },
            "water_outside_leak_under_home_temp_check": { "question": "Is the temperature below freezing or has it been below freezing in the last 72 hours?", "options": [ { "text": "Yes", "next": "water_outside_leak_under_home_frozen_time_check" }, { "text": "No", "next": "water_outside_leak_under_home_not_frozen_time_check" } ] },
            "water_outside_leak_under_home_frozen_time_check": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_outside_leak_under_home_frozen_after_hours" }, { "text": "No", "next": "water_outside_leak_under_home_frozen_during_hours" } ] },
            "water_outside_leak_under_home_frozen_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater” to “OFF”, and contact the warranty company for your water heater. Their number can be located in the “Warranty contact numbers” section under Topics of Interest” and they will schedule a certified technician to assist you. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to warranty@peakmhc.com with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_outside_leak_under_home_frozen_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_outside_leak_under_home_not_frozen_time_check": { "question": "Is it after 4PM or is it a weekend?", "options": [ { "text": "Yes", "next": "water_outside_leak_under_home_not_frozen_after_hours" }, { "text": "No", "next": "water_outside_leak_under_home_not_frozen_during_hours" } ] },
            "water_outside_leak_under_home_not_frozen_after_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Turn off water to home/water heater breaker; contact technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_outside_leak_under_home_not_frozen_during_hours": { "decision": { "text": "Turn the water off to the home immediately, flip the breaker in the main breaker panel that is labeled “Water Heater”, and send an email to **warranty@peakmhc.com**. Please include as many details as possible along with photos, your lot number, and your contact information.", "action": "Turn off water to home/water heater breaker; email warranty@peakmhc.com with details and photos." } },
            "water_outside_sewer_smell_start": { "question": "What type of sewer smell are you experiencing?", "options": [ { "text": "There is sewer water coming from the underbelly of the home", "next": "water_outside_sewer_underbelly" }, { "text": "There is sewer water coming from the ground under or near my home", "next": "water_outside_sewer_ground" } ] },
            "water_outside_sewer_underbelly": { "decision": { "text": "Contact a **certified technician**. If the technician feels the issue is a warranty issue, they will need to document well with before, during, and after photos. Submit the photos to **warranty@peakmhc.com** with as many details about the issue as possible. Please include your lot number and contact information in the email with any invoice you may have paid. It will then be reviewed to determine responsibility.", "action": "Contact certified technician. If warranty, document and submit to warranty@peakmhc.com." } },
            "water_outside_sewer_ground": { "decision": { "text": "Send an email to **warranty@peakmhc.com** with as many details about the issue as possible, please include photos. Please include your lot number and contact information in the email.", "action": "Email warranty@peakmhc.com with details, photos, lot number, and contact info." } },
            "structural_start": { "question": "What kind of structural issue are you experiencing?", "options": [ { "text": "Cracks in walls or ceiling", "next": "structural_wall_ceiling_crack_type" }, { "text": "Squeaky floors or subfloor issues", "next": "structural_floor_squeaks" }, { "text": "Doors or windows sticking/not closing properly", "next": "structural_door_window_sticking" }, { "text": "Foundation concerns (e.g., visible cracks, shifting)", "next": "structural_foundation_concerns" } ] },
            "structural_wall_ceiling_crack_type": { "question": "What does the crack look like?", "options": [ { "text": "Hairline cracks, small, not expanding", "next": "structural_wall_ceiling_crack_minor" }, { "text": "Large, jagged, or expanding cracks; cracks near doors/windows; cracks in brick/foundation", "next": "structural_wall_ceiling_crack_major" } ] },
            "structural_wall_ceiling_crack_minor": { "question": "Minor hairline cracks are common in new homes due to settling and humidity changes and are considered cosmetic and are typically not a covered issue after the initial move-in period. Monitor for expansion. If they expand significantly, or appear with other signs of structural movement, contact your warranty department at warranty@peakmhc.com if you have owned your home for les than 30 days. If you have owned your home for more than 30 days select the manufacturer of your home below. You can find this information on the data sheet located under your kitchen sink.", "options": [ { "text": "Clayton Waco 1", "next": "clayton_waco_1_contact" }, { "text": "Clayton Athens", "next": "clayton_athens_contact" }, { "text": "Clayton Southern Energy", "next": "clayton_southern_energy_contact" }, { "text": "Clayton Bonham", "next": "clayton_bonham_contact" }, { "text": "Clayton Sulpher Springs", "next": "clayton_sulphur_springs_contact" }, { "text": "New Vision", "next": "new_vision_contact" }, { "text": "Oak Creek", "next": "oak_creek_contact" }, { "text": "Fleetwood/CAVCO", "next": "fleetwood_cavco_contact" }, { "text": "Champion Homes", "next": "champion_homes_contact" } ] },
            "structural_wall_ceiling_crack_major": { "decision": { "text": "Large, expanding, or jagged cracks, especially near openings or in the foundation/brick, can indicate structural issues. Take photos and contact your warranty department at warranty@peakmhc.com immediately for assessment.", "action": "Take photos; contact warranty department immediately." } },
            "structural_floor_squeaks": { "question": "Squeaky floors can be caused by subfloor movement, loose nails, or changes in humidity. Minor squeaks are often present during the first couple of months of living in your newly manufactured home and are not viewed as a serious issue. If the squeaks are excessive or last more than a few weeks, contact your warranty department per your home manufacturer for an assessment. You can find this information on the data sheet located under your kitchen sink.", "options": [ { "text": "Clayton Waco 1", "next": "clayton_waco_1_contact" }, { "text": "Clayton Athens", "next": "clayton_athens_contact" }, { "text": "Clayton Southern Energy", "next": "clayton_southern_energy_contact" }, { "text": "Clayton Bonham", "next": "clayton_bonham_contact" }, { "text": "Clayton Sulpher Springs", "next": "clayton_sulphur_springs_contact" }, { "text": "New Vision", "next": "new_vision_contact" }, { "text": "Oak Creek", "next": "oak_creek_contact" }, { "text": "Fleetwood/CAVCO", "next": "fleetwood_cavco_contact" }, { "text": "Champion Homes", "next": "champion_homes_contact" } ] },
            "structural_door_window_sticking": { "question": "Doors or windows sticking can be due to humidity changes causing expansion, or minor settling of the home. Adjustments to hinges or frames may be needed. If the sticking is severe, sudden, or accompanied by other signs of structural movement (e.g., large cracks), contact your warranty department per your home manufacturer for an assessment. You can find this information on the data sheet located under your kitchen sink.", "options": [ { "text": "Clayton Waco 1", "next": "clayton_waco_1_contact" }, { "text": "Clayton Athens", "next": "clayton_athens_contact" }, { "text": "Clayton Southern Energy", "next": "clayton_southern_energy_contact" }, { "text": "Clayton Bonham", "next": "clayton_bonham_contact" }, { "text": "Clayton Sulpher Springs", "next": "clayton_sulphur_springs_contact" }, { "text": "New Vision", "next": "new_vision_contact" }, { "text": "Oak Creek", "next": "oak_creek_contact" }, { "text": "Fleetwood/CAVCO", "next": "fleetwood_cavco_contact" }, { "text": "Champion Homes", "next": "champion_homes_contact" } ] },
            "structural_foundation_concerns": {
                "question": "Large, expanding, or jagged cracks, especially near openings or in the foundation/brick, can indicate structural issues. Take photos and contact your warranty department at warranty@peakmhc.com immediately for assessment if you have owned your home for les than 30 days. If you have owned your home for more than 30 days select the manufacturer of your home below. You can find this information on the data sheet located under your kitchen sink.",
                "options": [
                    { "text": "Clayton Waco 1", "next": "clayton_waco_1_contact" },
                    { "text": "Clayton Athens", "next": "clayton_athens_contact" },
                    { "text": "Clayton Southern Energy", "next": "clayton_southern_energy_contact" },
                    { "text": "Clayton Bonham", "next": "clayton_bonham_contact" },
                    { "text": "Clayton Sulpher Springs", "next": "clayton_sulphur_springs_contact" },
                    { "text": "New Vision", "next": "new_vision_contact" },
                    { "text": "Oak Creek", "next": "oak_creek_contact" },
                    { "text": "Fleetwood/CAVCO", "next": "fleetwood_cavco_contact" },
                    { "text": "Champion Homes", "next": "champion_homes_contact" }
                ]
            },
            "clayton_waco_1_contact": {
                "decision": {
                    "text": "Clayton Waco 1, 6800 Imperial Dr, Waco TX 76712, 254-776-7777, connectservicewaco1@claytonhomes.com"
                }
            },
            "clayton_athens_contact": {
                "decision": {
                    "text": "Clayton Athens, 3429 State Hwy 31-West, Athens TX 75751, 903-386-3952,  connectserviceAthens@claytonhomes.com"
                }
            },
            "clayton_southern_energy_contact": {
                "decision": {
                    "text": "Clayton SE Texas, 8701 Harmon Rd, Fort Worth TX 76177 817-847-1355, connectserviceSEFtWorth@claytonhomes.com"
                }
            },
            "clayton_bonham_contact": {
                "decision": {
                    "text": "Clayton Bonham, 333 Austin St, Bonham TX 75418, 903-583-1949, connectservicebonham@claytonhomes.com"
                }
            },
            "clayton_sulphur_springs_contact": {
                "decision": {
                    "text": "Clayton Sulpher Springs, 2600 Main St, Sulphur Springs TX 75482, 903-439-0242, connectserviceSulphurSprings@claytonhomes.com"
                }
            },
            "new_vision_contact": {
                "decision": {
                    "text": "New Vision Homes, 1000 N. Industrial Rd, Madill OK 73446, 580-677-9937."
                }
            },
            "oak_creek_contact": {
                "decision": {
                    "text": "Oak Cree Homes, Lancaster TX, 972-230-3995"
                }
            },
            "champion_homes_contact": {
                "decision": {
                    "text": "Champion Homes, Burleson TX 76028, 877-201-3870"
                }
            },
            "fleetwood_cavco_contact": {
                "decision": {
                    "text": "fleetwoodhomes.com/contact/homeowner-service"
                }
            },
            "interior_start": { 
                "question": "What is the issue with your interior?",
                "options": [
                    { "text": "Damaged Linoleum", "next": "interior_linoleum" },
                    { "text": "Mini Blinds", "next": "interior_mini_blinds" },
                    { "text": "Loose or damaged trim or batten strips", "next": "interior_trim_strips" },
                    { "text": "Doors not closing", "next": "structural_door_window_sticking" },
                    { "text": "Damaged Wallboards", "next": "structural_wall_ceiling_crack_type" }
                ]
            },
            "interior_linoleum": {
                "question": "Tears in the linoleum are typically repaired in several ways.\n\n1. The tear is glued down to minimize curling of the floor to prevent additional damage.\n\n2. A section is removed and matched with another piece of linoleum. The new piece is then glued into place but may show wear sooner than if the repair was done as in option one.\n\n3. In severe cases the entire room is removed and replaced. This will usually include a border trim of quarter round and thresholds at any entry ways.\n\nIf you have lived in your home for less than 30 days contact your warranty department at warranty@peakmhc.com. If you have owned your home for more than 30 days you can reach out to the manufacturer of your home. This information can be found on the data sheet located under your kitchen cabinet. \n\nPlease keep in mind that flooring is not generally a covered item and you as the homeowner may be responsible for any repairs to the flooring. \n\nWhat is the manufacturer of your home?",
                "options": [
                    { "text": "Clayton Waco 1", "next": "clayton_waco_1_contact" },
                    { "text": "Clayton Athens", "next": "clayton_athens_contact" },
                    { "text": "Clayton Southern Energy", "next": "clayton_southern_energy_contact" },
                    { "text": "Clayton Bonham", "next": "clayton_bonham_contact" },
                    { "text": "Clayton Sulpher Springs", "next": "clayton_sulphur_springs_contact" },
                    { "text": "New Vision", "next": "new_vision_contact" },
                    { "text": "Oak Creek", "next": "oak_creek_contact" },
                    { "text": "Fleetwood/CAVCO", "next": "fleetwood_cavco_contact" },
                    { "text": "Champion Homes", "next": "champion_homes_contact" }
                ]
            },
            "interior_mini_blinds": { "decision": { "text": "Mini blinds are not a covered item after the initial 30 days of owning your new home. Any cost incurred in repairing or replacing mini blinds after this initial 30 days is at the homeowners expense." } },
            "interior_trim_strips": { "decision": { "text": "Batten strips and trim pieces are not a covered item after the initial 30 days of owning your new home. Any cost incurred in repairing or replacing mini blinds after this initial 30 days is at the homeowners expense." } },
            
            "yard_community_check": {
                "question": "Which Community do you live in?",
                "options": [
                    { "text": "Rancho Serenidad", "next": "yard_rancho_start" },
                    { "text": "Villas De Mariposa", "next": "yard_villas_start" },
                    { "text": "Brazos Point", "next": "yard_brazos_start" },
                    { "text": "Solena", "next": "yard_solena_start" }
                ]
            },
            "yard_rancho_start": { "question": "What kind of yard-related issue is it?", "options": [ { "text": "Drainage/Water pooling issues", "next": "yard_drainage_rancho" }, { "text": "Landscaping/Sod concerns", "next": "yard_landscaping_rancho" }, { "text": "Fence damage/issues", "next": "yard_fence_rancho" }, { "text": "Sprinkler system problem", "next": "yard_sprinkler_rancho" } ] },
            "yard_villas_start": { "question": "What kind of yard-related issue is it?", "options": [ { "text": "Drainage/Water pooling issues", "next": "yard_drainage_villas" }, { "text": "Landscaping/Sod concerns", "next": "yard_landscaping_villas" }, { "text": "Fence damage/issues", "next": "yard_fence_villas" }, { "text": "Sprinkler system problem", "next": "yard_sprinkler_villas" } ] },
            "yard_brazos_start": { "question": "What kind of yard-related issue is it?", "options": [ { "text": "Drainage/Water pooling issues", "next": "yard_drainage_brazos" }, { "text": "Landscaping/Sod concerns", "next": "yard_landscaping_brazos" }, { "text": "Fence damage/issues", "next": "yard_fence_brazos" }, { "text": "Sprinkler system problem", "next": "yard_sprinkler_brazos" } ] },
            "yard_solena_start": { "question": "What kind of yard-related issue is it?", "options": [ { "text": "Drainage/Water pooling issues", "next": "yard_drainage_solena" }, { "text": "Landscaping/Sod concerns", "next": "yard_landscaping_solena" }, { "text": "Fence damage/issues", "next": "yard_fence_solena" }, { "text": "Sprinkler system problem", "next": "yard_sprinkler_solena" } ] },
            "yard_drainage_rancho": { "decision": { "text": "Ensure downspouts are properly directed away from the home and that gutters are clear. Observe during rainfall to identify precise areas of pooling. Minor surface pooling may be normal after heavy rain. If water consistently pools near the home or causes significant erosion, contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_landscaping_rancho": { "decision": { "text": "Any issues with the landscaping or Sod should be directed to the property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_fence_rancho": { "decision": { "text": "If there are issues with the fencing, please indicate the location in the community when contacting your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_sprinkler_rancho": { "decision": { "text": "Any issues with the sprinkler system such as a busted sprinkler head or standing water around a sprinkle head please contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_drainage_villas": { "decision": { "text": "Ensure downspouts are properly directed away from the home and that gutters are clear. Observe during rainfall to identify precise areas of pooling. Minor surface pooling may be normal after heavy rain. If water consistently pools near the home or causes significant erosion, contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_landscaping_villas": { "decision": { "text": "Any issues with the landscaping or Sod should be directed to the property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_fence_villas": { "decision": { "text": "If there are issues with the fencing, please indicate the location in the community when contacting your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_sprinkler_villas": { "decision": { "text": "Any issues with the sprinkler system such as a busted sprinkler head or standing water around a sprinkle head please contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_drainage_brazos": { "decision": { "text": "Ensure downspouts are properly directed away from the home and that gutters are clear. Observe during rainfall to identify precise areas of pooling. Minor surface pooling may be normal after heavy rain. If water consistently pools near the home or causes significant erosion, contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_landscaping_brazos": { "decision": { "text": "Any issues with the landscaping or Sod should be directed to the property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_fence_brazos": { "decision": { "text": "If there are issues with the fencing, please indicate the location in the community when contacting your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_sprinkler_brazos": { "decision": { "text": "Any issues with the sprinkler system such as a busted sprinkler head or standing water around a sprinkle head please contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_drainage_solena": { "decision": { "text": "Ensure downspouts are properly directed away from the home and that gutters are clear. Observe during rainfall to identify precise areas of pooling. Minor surface pooling may be normal after heavy rain. If water consistently pools near the home or causes significant erosion, contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_landscaping_solena": { "decision": { "text": "Any issues with the landscaping or Sod should be directed to the property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_fence_solena": { "decision": { "text": "If there are issues with the fencing, please indicate the location in the community when contacting your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email." } },
            "yard_sprinkler_solena": { "decision": { "text": "Any issues with the sprinkler system such as a busted sprinkler head or standing water around a sprinkle head please contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email." } },
            
            "community_community_check": {
                "question": "Which Community do you live in?",
                "options": [
                    { "text": "Rancho", "next": "community_start_rancho" },
                    { "text": "Villas", "next": "community_start_villas" },
                    { "text": "Brazos", "next": "community_start_brazos" },
                    { "text": "Solena", "next": "community_start_solena" }
                ]
            },
            "community_start_rancho": { "question": "What kind of community-related issue is it?", "options": [ { "text": "Street light out/damaged", "next": "community_street_light_issue_rancho" }, { "text": "Amenity damage/issue (e.g., pool, clubhouse)", "next": "community_amenity_damage_rancho" }, { "text": "Community violation/concern", "next": "community_hoa_violation_rancho" }, { "text": "Common area maintenance (e.g., park, green space)", "next": "community_common_area_maintenance_rancho" } ] },
            "community_start_villas": { "question": "What kind of community-related issue is it?", "options": [ { "text": "Street light out/damaged", "next": "community_street_light_issue_villas" }, { "text": "Amenity damage/issue (e.g., pool, clubhouse)", "next": "community_amenity_damage_villas" }, { "text": "Community violation/concern", "next": "community_hoa_violation_villas" }, { "text": "Common area maintenance (e.g., park, green space)", "next": "community_common_area_maintenance_villas" } ] },
            "community_start_brazos": { "question": "What kind of community-related issue is it?", "options": [ { "text": "Street light out/damaged", "next": "community_street_light_issue_brazos" }, { "text": "Amenity damage/issue (e.g., pool, clubhouse)", "next": "community_amenity_damage_brazos" }, { "text": "Community violation/concern", "next": "community_hoa_violation_brazos" }, { "text": "Common area maintenance (e.g., park, green space)", "next": "community_common_area_maintenance_brazos" } ] },
            "community_start_solena": { "question": "What kind of community-related issue is it?", "options": [ { "text": "Street light out/damaged", "next": "community_street_light_issue_solena" }, { "text": "Amenity damage/issue (e.g., pool, clubhouse)", "next": "community_amenity_damage_solena" }, { "text": "Community violation/concern", "next": "community_hoa_violation_solena" }, { "text": "Common area maintenance (e.g., park, green space)", "next": "community_common_area_maintenance_solena" } ] },
            "community_street_light_issue_rancho": { "decision": { "text": "Report street light outages or damage directly to the Park office and please provide accurate location information. Contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Contact property management." } },
            "community_amenity_damage_rancho": { "question": "Which amenity is damaged?", "options": [ { "text": "Pool/Splash Pad", "next": "community_amenity_pool_rancho" }, { "text": "Playground", "next": "community_amenity_playground_rancho" }, { "text": "Common Area Landscaping/Fencing", "next": "community_amenity_landscaping_rancho" }, { "text": "Clubhouse/Fitness Center", "next": "community_amenity_clubhouse_rancho" }, { "text": "Walking Trails/Sidewalks", "next": "community_amenity_trails_rancho" } ] },
            "community_amenity_pool_rancho": { "decision": { "text": "Report issues with community pools or splash pads directly to the property management. Contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report property management." } },
            "community_amenity_playground_rancho": { "decision": { "text": "Report any damage or safety concerns at community playgrounds to the property management company immediately. Provide photos if possible. Contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_landscaping_rancho": { "decision": { "text": "For issues with common area landscaping, such as dead plants, irrigation leaks, or damaged fencing, report them to property management. Contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_clubhouse_rancho": { "decision": { "text": "Report maintenance issues, damage, or concerns about the clubhouse or fitness center facilities to property management. Contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_trails_rancho": { "decision": { "text": "Report damage, blockages, or safety concerns regarding community walking trails or sidewalks to the property management. Contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_hoa_violation_rancho": { "decision": { "text": "For community violations please contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_common_area_maintenance_rancho": { "decision": { "text": "Report general maintenance needs or concerns in common areas (parks, green spaces, entryways) to property management. Contact your property management team at manager@ranchoserenidad.com or 817-455-6335 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_street_light_issue_villas": { "decision": { "text": "Report street light outages or damage directly to the Park office and please provide accurate location information. Contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Contact property management." } },
            "community_amenity_damage_villas": { "question": "Which amenity is damaged?", "options": [ { "text": "Pool/Splash Pad", "next": "community_amenity_pool_villas" }, { "text": "Playground", "next": "community_amenity_playground_villas" }, { "text": "Common Area Landscaping/Fencing", "next": "community_amenity_landscaping_villas" }, { "text": "Clubhouse/Fitness Center", "next": "community_amenity_clubhouse_villas" }, { "text": "Walking Trails/Sidewalks", "next": "community_amenity_trails_villas" } ] },
            "community_amenity_pool_villas": { "decision": { "text": "Report issues with community pools or splash pads directly to the property management. Contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report property management." } },
            "community_amenity_playground_villas": { "decision": { "text": "Report any damage or safety concerns at community playgrounds to the property management company immediately. Provide photos if possible. Contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_landscaping_villas": { "decision": { "text": "For issues with common area landscaping, such as dead plants, irrigation leaks, or damaged fencing, report them to property management. Contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_clubhouse_villas": { "decision": { "text": "Report maintenance issues, damage, or concerns about the clubhouse or fitness center facilities to property management. Contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_trails_villas": { "decision": { "text": "Report damage, blockages, or safety concerns regarding community walking trails or sidewalks to the property management. Contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_hoa_violation_villas": { "decision": { "text": "For community violations please contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_common_area_maintenance_villas": { "decision": { "text": "Report general maintenance needs or concerns in common areas (parks, green spaces, entryways) to property management. Contact your property management team at manager@villasdemariposas.com or (817) 783-0590 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_street_light_issue_brazos": { "decision": { "text": "Report street light outages or damage directly to the Park office and please provide accurate location information. Contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Contact property management." } },
            "community_amenity_damage_brazos": { "question": "Which amenity is damaged?", "options": [ { "text": "Pool/Splash Pad", "next": "community_amenity_pool_brazos" }, { "text": "Playground", "next": "community_amenity_playground_brazos" }, { "text": "Common Area Landscaping/Fencing", "next": "community_amenity_landscaping_brazos" }, { "text": "Clubhouse/Fitness Center", "next": "community_amenity_clubhouse_brazos" }, { "text": "Walking Trails/Sidewalks", "next": "community_amenity_trails_brazos" } ] },
            "community_amenity_pool_brazos": { "decision": { "text": "Report issues with community pools or splash pads directly to the property management. Contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report property management." } },
            "community_amenity_playground_brazos": { "decision": { "text": "Report any damage or safety concerns at community playgrounds to the property management company immediately. Provide photos if possible. Contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_landscaping_brazos": { "decision": { "text": "For issues with common area landscaping, such as dead plants, irrigation leaks, or damaged fencing, report them to property management. Contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_clubhouse_brazos": { "decision": { "text": "Report maintenance issues, damage, or concerns about the clubhouse or fitness center facilities to property management. Contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_trails_brazos": { "decision": { "text": "Report damage, blockages, or safety concerns regarding community walking trails or sidewalks to the property management. Contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_hoa_violation_brazos": { "decision": { "text": "For community violations please contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_common_area_maintenance_brazos": { "decision": { "text": "Report general maintenance needs or concerns in common areas (parks, green spaces, entryways) to property management. Contact your property management team at manager@brazospoint.com or (281) 473-0080 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_street_light_issue_solena": { "decision": { "text": "Report street light outages or damage directly to the Park office and please provide accurate location information. Contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Contact property management." } },
            "community_amenity_damage_solena": { "question": "Which amenity is damaged?", "options": [ { "text": "Pool/Splash Pad", "next": "community_amenity_pool_solena" }, { "text": "Playground", "next": "community_amenity_playground_solena" }, { "text": "Common Area Landscaping/Fencing", "next": "community_amenity_landscaping_solena" }, { "text": "Clubhouse/Fitness Center", "next": "community_amenity_clubhouse_solena" }, { "text": "Walking Trails/Sidewalks", "next": "community_amenity_trails_solena" } ] },
            "community_amenity_pool_solena": { "decision": { "text": "Report issues with community pools or splash pads directly to the property management. Contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report property management." } },
            "community_amenity_playground_solena": { "decision": { "text": "Report any damage or safety concerns at community playgrounds to the property management company immediately. Provide photos if possible. Contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_landscaping_solena": { "decision": { "text": "For issues with common area landscaping, such as dead plants, irrigation leaks, or damaged fencing, report them to property management. Contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_clubhouse_solena": { "decision": { "text": "Report maintenance issues, damage, or concerns about the clubhouse or fitness center facilities to property management. Contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_amenity_trails_solena": { "decision": { "text": "Report damage, blockages, or safety concerns regarding community walking trails or sidewalks to the property management. Contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_hoa_violation_solena": { "decision": { "text": "For community violations please contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            "community_common_area_maintenance_solena": { "decision": { "text": "Report general maintenance needs or concerns in common areas (parks, green spaces, entryways) to property management. Contact your property management team at manager@solena.com or (972) 970-0010 for assistance. Make sure to include photos and your information if you are sending an email.", "action": "Report to property management." } },
            
            // Placeholder nodes for new categories
            "exterior_start": { 
                "question": "What is the issue with the exterior?",
                "options": [
                    { "text": "Decking", "next": "exterior_decking" },
                    { "text": "Walls", "next": "exterior_walls" },
                    { "text": "Skirting", "next": "exterior_skirting" }
                ]
            },
            "exterior_decking": { "decision": { "text": "Information regarding decking issues is currently under development. Please check back later." } },
            "exterior_walls": { "decision": { "text": "Information regarding exterior wall issues is currently under development. Please check back later." } },
            "exterior_skirting": { "decision": { "text": "Information regarding skirting issues is currently under development. Please check back later." } },
            "vendors_placeholder": { 
                "decision": { 
                    "text": `These vendors are only recommendations; you are not required to utilize their services. Peak assumes no responsibility for the work performed or charges for those services by any of the following vendors, they are merely recommendations. 
PLUMBERS
Plumbaholics 214-695-2402
ELECTRICIANS
ROOFERS
Buffalo Trail Construction 817-995-9525
HANDYMAN SERVICES
345 Handyman Services 682-374-8261`
                } 
            },
            "contacts_placeholder": { 
                "decision": { 
                    "text": `APPLIANCES
Samsung 	800-726-7864
Whirlpool	800-253-1301
Frigidaire	888-307-2326
Electrolux	888-307-3236
Hunter		800-715-0393
First Alert	800-323-9005
HVAC
Champion	682-230-5566
Mr. Chill	        936-236-9241
Nordyne	        800-422--4328
Stylecrest	800-231-4822
Carrier		866-234-1018
Ecobee	        877-932-6233
WATER HEATER
Rheem	        800-432-8373 Without Heat Pump
Rheem	        800-995-0982 With Heat Pump
FAUCETS
Moen		800-289-6636
CFG		        888-450-5522
BBC		        866-427-5001
Pfister		800-732-8328`
                } 
            },
            "not_listed_placeholder": { "decision": { "text": "If you were not able to find your issue within this tool please send an email to warranty@peakmhc.com. Please include; Your Name, address, a contact number, Lot number, the community in which you live, the issue you are having with as much description as possible and any steps you have already taken to remedy the issue, and photos." } },
            // =================== APPLIANCE / HVAC SECTION (MODIFIED) ===================
            "appliance_hvac_start": { "question": "What kind of appliance or HVAC issue is it?", "options": [ { "text": "HVAC (Heating, Ventilation, Air Conditioning)", "next": "hvac_start" }, { "text": "Refrigerator", "next": "refrigerator_start_new" }, { "text": "Stove/Cook Top/Hood Vent", "next": "stove_start_new" }, { "text": "Microwave", "next": "microwave_start_new" }, { "text": "Dishwasher", "next": "dishwasher_start_new" }, { "text": "Water Heater (appliance related)", "next": "water_heater_start_new" } ] },
            
            // HVAC Community Final Decisions (NEW)
            "hvac_decision_champion": {
                "decision": {
                    "text": "Contact Champion Heating and Air for service to your HVAC unit. Their contact number is 682-230-5566 and their email is installservice@championheatingair.net. When contacting them you will need to reference your address with your lot number, your contact information, and the serial number which you can find located on the data sheet under your kitchen sink."
                }
            },
            "hvac_decision_mr_chill": {
                "decision": {
                    "text": "Contact Mr. Chill Heating and Air  for service to your HVAC unit. Their contact number is 936-236-9241 and their email is office@mrchillcoolingandheating.com. When contacting them you will need to reference your address with your lot number, your contact information, and the serial number which you can find located on the data sheet under your kitchen sink."
                }
            },
            
            // HVAC Path (MODIFIED)
            "hvac_start": { "question": "What is the HVAC issue?", "options": [ { "text": "Not cooling/heating effectively", "next": "hvac_not_cooling_heating" }, { "text": "No power/Not turning on", "next": "hvac_no_power" }, { "text": "Poor airflow from vents", "next": "hvac_poor_airflow" }, { "text": "Unusual noises or smells", "next": "hvac_noise_smell" } ] },
            "hvac_not_cooling_heating": { "question": "Have you checked the thermostat settings and replaced/cleaned your air filter recently?", "options": [ { "text": "Yes, thermostat is correct and filter is clean/new.", "next": "hvac_cooling_heating_filter_thermostat_ok" }, { "text": "No, I haven't, or they are incorrect/dirty.", "next": "hvac_thermostat_filter_action" } ] },
            "hvac_thermostat_filter_action": { "decision": { "text": "Adjust thermostat settings (ensure it's on 'Cool' or 'Heat' and 'Auto' for fan). Clean or replace your air filter. A dirty filter severely restricts performance. If the issue persists after these steps, contact your **warranty department** or a **qualified HVAC technician**.", "action": "Adjust thermostat/clean filter; if persists, contact warranty/HVAC tech." } },
            "hvac_cooling_heating_filter_thermostat_ok": { "question": "Is the outdoor unit (condenser) running, and is it free of debris?", "options": [ { "text": "Yes, unit is running and clear.", "next": "hvac_outdoor_unit_ok" }, { "text": "No, unit is not running or is obstructed.", "next": "hvac_outdoor_unit_issue" } ] },
            "hvac_outdoor_unit_ok": { 
                "question": "If the outdoor unit is running and clear, but there's no effective cooling/heating, it could indicate a refrigerant issue, a faulty compressor, or an indoor coil problem. What Community do you live in?",
                "options": [
                    { "text": "Rancho", "next": "hvac_decision_champion" },
                    { "text": "Villas", "next": "hvac_decision_champion" },
                    { "text": "Brazos", "next": "hvac_decision_mr_chill" },
                    { "text": "Solena", "next": "hvac_decision_champion" }
                ]
            },
            "hvac_outdoor_unit_issue": { 
                "question": "Ensure the outdoor unit is free of leaves, dirt, or other obstructions. If it's not running, check the circuit breaker. If clearing debris or resetting the breaker doesn't help, or if the unit is frozen, What Community do you live in?",
                "options": [
                    { "text": "Rancho", "next": "hvac_decision_champion" },
                    { "text": "Villas", "next": "hvac_decision_champion" },
                    { "text": "Brazos", "next": "hvac_decision_mr_chill" },
                    { "text": "Solena", "next": "hvac_decision_champion" }
                ]
            },
            "hvac_no_power": { "question": "Have you checked the circuit breaker for your HVAC system in the main electrical panel?", "options": [ { "text": "Yes, the breaker is tripped or off.", "next": "hvac_breaker_tripped" }, { "text": "No, the breaker is on.", "next": "hvac_no_breaker_issue" } ] },
            "hvac_breaker_tripped": {
                "question": "Reset the breaker. If it immediately trips again, there's a serious electrical issue. If it stays on but the unit still doesn't power up, there is another issue. In either case, What Community do you live in?",
                 "options": [
                    { "text": "Rancho", "next": "hvac_decision_champion" },
                    { "text": "Villas", "next": "hvac_decision_champion" },
                    { "text": "Brazos", "next": "hvac_decision_mr_chill" },
                    { "text": "Solena", "next": "hvac_decision_champion" }
                ]
            },
            "hvac_no_breaker_issue": {
                "question": "If the breaker is on, but the HVAC system has no power, the issue could be with the thermostat, control board, or other electrical components within the unit. What Community do you live in?",
                "options": [
                    { "text": "Rancho", "next": "hvac_decision_champion" },
                    { "text": "Villas", "next": "hvac_decision_champion" },
                    { "text": "Brazos", "next": "hvac_decision_mr_chill" },
                    { "text": "Solena", "next": "hvac_decision_champion" }
                ]
            },
            "hvac_poor_airflow": { "question": "Have you checked and replaced/cleaned your air filter recently?", "options": [ { "text": "Yes, the filter is clean/new.", "next": "hvac_airflow_filter_ok" }, { "text": "No, I haven't, or it's dirty.", "next": "hvac_filter_dirty_action" } ] },
            "hvac_filter_dirty_action": { 
                "decision": { 
                    "text": "Clean or replace your air filter immediately. A dirty filter severely restricts airflow and can damage your HVAC system. If the issue persists after replacing the filter, return to the previous step in this process."
                } 
            },
            "hvac_airflow_filter_ok": { "question": "Are all vents open and unobstructed by furniture or curtains?", "options": [ { "text": "Yes, all vents are open and clear.", "next": "hvac_airflow_vents_clear" }, { "text": "No, some vents were closed/blocked.", "next": "hvac_airflow_vents_adjusted" } ] },
            "hvac_airflow_vents_adjusted": { 
                "decision": { 
                    "text": "Ensure all supply and return vents are open and unobstructed. This can significantly impact airflow. If the issue persists, return to the previous step in this process."
                } 
            },
            "hvac_airflow_vents_clear": {
                "question": "If filters are clean and vents are clear, poor airflow could be due to duct leaks, a failing blower motor, or an issue with the evaporator coil. What Community do you live in?",
                "options": [
                    { "text": "Rancho", "next": "hvac_decision_champion" },
                    { "text": "Villas", "next": "hvac_decision_champion" },
                    { "text": "Brazos", "next": "hvac_decision_mr_chill" },
                    { "text": "Solena", "next": "hvac_decision_champion" }
                ]
            },
            "hvac_noise_smell": { "question": "What kind of noise or smell are you experiencing?", "options": [ { "text": "Loud banging, grinding, or screeching noises", "next": "hvac_noise_mechanical" }, { "text": "Musty, moldy, or burnt smells", "next": "hvac_smell_odor" } ] },
            "hvac_noise_mechanical": {
                "question": "These noises typically indicate a serious mechanical problem (e.g., motor bearing failure, loose fan blade, compressor issue). Shut off the unit immediately to prevent further damage. What Community do you live in?",
                "options": [
                    { "text": "Rancho", "next": "hvac_decision_champion" },
                    { "text": "Villas", "next": "hvac_decision_champion" },
                    { "text": "Brazos", "next": "hvac_decision_mr_chill" },
                    { "text": "Solena", "next": "hvac_decision_champion" }
                ]
            },
            "hvac_smell_odor": {
                "question": "Musty/moldy smells suggest moisture issues or mold in the ductwork/unit. Burnt smells could indicate electrical problems. For any persistent unusual smell, especially burning, shut off the unit. What Community do you live in?",
                "options": [
                    { "text": "Rancho", "next": "hvac_decision_champion" },
                    { "text": "Villas", "next": "hvac_decision_champion" },
                    { "text": "Brazos", "next": "hvac_decision_mr_chill" },
                    { "text": "Solena", "next": "hvac_decision_champion" }
                ]
            },
            // Other appliance sections
            "refrigerator_start_new": { "question": "What is the issue with your refrigerator?", "options": [ { "text": "It is not running", "next": "appliance_manufacturer_question" }, { "text": "The fridge is cool, but the freezer does not work", "next": "appliance_manufacturer_question" }, { "text": "There is an alarm indicating the filter needs to be changed", "next": "refrigerator_filter_alarm" }, { "text": "There is an alarm indicating it is not cooling properly", "next": "appliance_manufacturer_question" } ] },
            "refrigerator_filter_alarm": { "decision": { "text": "Remove the filter and locate the serial number on the filter. Purchase a new filter and replace it. This is a **homeowner’s responsibility**, and the homeowner will be responsible for changing the filter and any charges associated with changing the filter.", "action": "Homeowner responsibility: Purchase and replace filter." } },
            "stove_start_new": { "question": "What is the issue with your stove, cooktop, or hood vent?", "options": [ { "text": "A burner is not heating properly", "next": "appliance_manufacturer_question" }, { "text": "The oven does not work", "next": "appliance_manufacturer_question" }, { "text": "A piece of the stove has broken off", "next": "appliance_manufacturer_question" }, { "text": "The door will not close properly", "next": "appliance_manufacturer_question" }, { "text": "The surface is broken or cracked", "next": "appliance_manufacturer_question" }, { "text": "There is no power", "next": "decision_email_warranty_stove_power" } ] },
            "decision_email_warranty_stove_power": { "decision": { "text": "Send an email to **warranty@peakmhc.com**. Provide your contact information, the park you live in, and your lot number.", "action": "Email warranty with contact info, park, and lot number." } },
            "microwave_start_new": { "question": "What is the issue with your microwave?", "options": [ { "text": "The microwave is not heating", "next": "microwave_manufacturer_question" }, { "text": "The turn table does not turn", "next": "microwave_manufacturer_question" }, { "text": "The light does not come on", "next": "microwave_manufacturer_question" }, { "text": "There is no power", "next": "decision_email_warranty_microwave_power" }, { "text": "The door will not stay closed", "next": "microwave_manufacturer_question" } ] },
            "microwave_manufacturer_question": { "question": "Who is the manufacturer of your microwave?", "options": [ { "text": "Samsung", "next": "decision_discontinue_contact_samsung" }, { "text": "General Electric (GE)", "next": "decision_discontinue_contact_ge" }, { "text": "Whirlpool", "next": "decision_discontinue_contact_whirlpool" }, { "text": "Frigidaire", "next": "decision_discontinue_contact_frigidaire" }, { "text": "Electrolux", "next": "decision_discontinue_contact_electrolux" } ] },
            "decision_email_warranty_microwave_power": { "decision": { "text": "Send an email to **warranty@peakmhc.com**.", "action": "Email warranty." } },
            "dishwasher_start_new": { "question": "What is the issue with your dishwasher?", "options": [ { "text": "There is no power", "next": "decision_email_warranty_dishwasher" }, { "text": "It will not dry the dishes", "next": "dishwasher_manufacturer_question" }, { "text": "It will not complete a cycle", "next": "dishwasher_manufacturer_question" }, { "text": "It is loose from the cabinet", "next": "decision_email_warranty_dishwasher" }, { "text": "The door will not close", "next": "dishwasher_door_wont_close" }, { "text": "There is a leak", "next": "dishwasher_leak_question" } ] },
            "dishwasher_manufacturer_question": { "question": "Who is the manufacturer of your dishwasher?", "options": [ { "text": "Samsung", "next": "decision_discontinue_contact_samsung" }, { "text": "General Electric (GE)", "next": "decision_discontinue_contact_ge" }, { "text": "Whirlpool", "next": "decision_discontinue_contact_whirlpool" }, { "text": "Frigidaire", "next": "decision_discontinue_contact_frigidaire" }, { "text": "Electrolux", "next": "decision_discontinue_contact_electrolux" } ] },
            "decision_email_warranty_dishwasher": { "decision": { "text": "Discontinue use of the dishwasher and send an email to **warranty@peakmhc.com**. Include your contact information, lot number, and a brief description of the issue.", "action": "Email warranty with contact info, lot number, and description." } },
            "dishwasher_door_wont_close": { "question": "Why will the door not close?", "options": [ { "text": "The cabinet is in the way", "next": "decision_email_warranty_dishwasher" }, { "text": "It will not latch when closed all the way", "next": "dishwasher_manufacturer_question" } ] },
            "dishwasher_leak_question": { "question": "Where is the leak coming from?", "options": [ { "text": "It comes from under the dishwasher", "next": "dishwasher_manufacturer_question" }, { "text": "It comes from under the sink where the drain line connects", "next": "decision_email_warranty_dishwasher" } ] },
            "water_heater_start_new": { "question": "What is the issue with your water heater?", "options": [ { "text": "There is no hot water", "next": "water_heater_no_hot_water_q" }, { "text": "The water is too hot", "next": "water_heater_too_hot" }, { "text": "There is a leak", "next": "water_heater_leak_q" }, { "text": "The water heater is making a weird noise", "next": "decision_contact_rheem_updated" } ] },
            "water_heater_no_hot_water_q": { "question": "Does the water heater have power?", "options": [ { "text": "Yes", "next": "decision_contact_rheem_updated" }, { "text": "No", "next": "decision_email_warranty_water_heater" } ] },
            "water_heater_too_hot": { "decision": { "text": "A safe temperature to operate the water heater at is 131 degrees. If you are not able to set the temperature at this temperature you will need to contact Rheem at 800-432-8373 if your system does not have a heat pump or 800-995-0982 if your system has a heat pump. You will need the serial number of the appliance located on the data sheet under your kitchen sink. A heat pump is an additional section on top of the water heater tank with a blower and vents on the side of the blower housing.", "action": "Check temp; if unable to set, contact Rheem with serial number." } },
            "water_heater_leak_q": { "question": "Where is the leak coming from?", "options": [ { "text": "It is coming from one of the water lines", "next": "decision_email_warranty_water_heater" }, { "text": "It is coming from the water heater itself", "next": "water_heater_leak_from_unit" } ] },
            "water_heater_leak_from_unit": { "decision": { "text": "Turn the water off to the water heater and contact Rheem at 800-432-8373 if your system does not have a heat pump or 800-995-0982 if your system has a heat pump. You will need the serial number of the appliance located on the data sheet under your kitchen sink.", "action": "Turn off water to heater; contact Rheem with serial number." } },
            "decision_email_warranty_water_heater": { "decision": { "text": "Send an email to **warranty@peakmhc.com**. Include your contact information, lot number, and a brief description of the issue.", "action": "Email warranty with contact info, lot number, and description." } },
            "appliance_manufacturer_question": { "question": "Who is the manufacturer of your appliance?", "options": [ { "text": "Samsung", "next": "decision_contact_samsung" }, { "text": "General Electric (GE)", "next": "decision_contact_ge" }, { "text": "Whirlpool", "next": "decision_contact_whirlpool" }, { "text": "Frigidaire", "next": "decision_contact_frigidaire" }, { "text": "Electrolux", "next": "decision_contact_electrolux" } ] },
            "decision_contact_samsung": { "decision": { "text": "Contact Samsung at **800-726-7864**. You will need the serial number of the appliance. This information can be found on the data sheet located under your kitchen sink. You will also need to provide the service department with your address and Lot number as well as your contact information. Document who you spoke with and make sure to get a case number. You, as the homeowner, are the necessary first contact with the warranty department for the repair of your appliance. If the Peak Enterprises Warranty department needs to get involved, they will need all of this information to assist you in getting the needed repairs." } },
            "decision_contact_ge": { "decision": { "text": "Contact GE at **800-432-2737**. You will need the serial number of the appliance. This information can be found on the data sheet located under your kitchen sink. You will also need to provide the service department with your address and Lot number as well as your contact information. Document who you spoke with and make sure to get a case number. You, as the homeowner, are the necessary first contact with the warranty department for the repair of your appliance. If the Peak Enterprises Warranty department needs to get involved, they will need all of this information to assist you in getting the needed repairs." } },
            "decision_contact_whirlpool": { "decision": { "text": "Contact Whirlpool at **866-698-2538**. You will need the serial number of the appliance. This information can be found on the data sheet located under your kitchen sink. You will also need to provide the service department with your address and Lot number as well as your contact information. Document who you spoke with and make sure to get a case number. You, as the homeowner, are the necessary first contact with the warranty department for the repair of your appliance. If the Peak Enterprises Warranty department needs to get involved, they will need all of this information to assist you in getting the needed repairs." } },
            "decision_contact_frigidaire": { "decision": { "text": "Contact Frigidaire at **866-386-5286**. You will need the serial number of the appliance. This information can be found on the data sheet located under your kitchen sink. You will also need to provide the service department with your address and Lot number as well as your contact information. Document who you spoke with and make sure to get a case number. You, as the homeowner, are the necessary first contact with the warranty department for the repair of your appliance. If the Peak Enterprises Warranty department needs to get involved, they will need all of this information to assist you in getting the needed repairs." } },
            "decision_contact_electrolux": { "decision": { "text": "Discontinue use of the appliance and contact Electrolux at **866-386-5286**. You will need the serial number of the appliance. This information can be found on the data sheet located under your kitchen sink. You will also need to provide the service department with your address and Lot number as well as your contact information. Document who you spoke with and make sure to get a case number. You, as the homeowner, are the necessary first contact with the warranty department for the repair of your appliance. If the Peak Enterprises Warranty department needs to get involved, they will need all of this information to assist you in getting the needed repairs." } }
        };
        // --- Pre-process flowTree to assign unique IDs to all options ---
        let uniqueIdCounter = 1;
        for (const nodeId in flowTree) {
            if (flowTree.hasOwnProperty(nodeId) && flowTree[nodeId].options) {
                flowTree[nodeId].options.forEach(option => {
                    option.uid = uniqueIdCounter++;
                });
            }
        }
        // ----------------------------------------------------------------
        let currentPath = []; // To keep track of the user's path for the back button
        const flowContentDiv = document.getElementById('flowContent');
        const resetButton = document.getElementById('resetButton');
        const disclaimerOverlay = document.getElementById('disclaimer-overlay');
        const acceptButton = document.getElementById('accept-disclaimer-btn');
        // Function to go back to the previous node
        function goBack() {
            if (currentPath.length > 1) {
                currentPath.pop(); // Remove the current node from history
                const previousNodeId = currentPath[currentPath.length - 1]; // Get the new last node
                renderNode(previousNodeId, false); // false to not re-add to history
            }
        }
        
        // Function to render a node from the flowTree
        function renderNode(nodeId, addToHistory = true) {
            if (addToHistory) {
                // To prevent duplicates when using the back button
                if (currentPath[currentPath.length - 1] !== nodeId) {
                    currentPath.push(nodeId);
                }
            } else {
                // If not adding to history, it means we are going back, so ensure the path is correct
                currentPath.pop();
                currentPath.push(nodeId);
            }
            
            const node = flowTree[nodeId];
            if (!node) {
                flowContentDiv.innerHTML = `<p class="decision-text text-red-600">Error: Path not found for ID: ${nodeId}</p>`;
                resetButton.classList.remove('hidden');
                return;
            }
            flowContentDiv.innerHTML = ''; // Clear previous content
            // Add back button if there's history beyond the first question
            if (currentPath.length > 1) {
                const backButton = document.createElement('button');
                backButton.className = 'back-button';
                backButton.textContent = '← Back';
                backButton.onclick = goBack;
                flowContentDiv.appendChild(backButton);
            }
            if (node.question) {
                // Display question
                const questionElement = document.createElement('p');
                questionElement.className = 'question-text mb-6';
                questionElement.textContent = node.question;
                flowContentDiv.appendChild(questionElement);
                // Display options as buttons
                const buttonGrid = document.createElement('div');
                buttonGrid.className = 'button-grid';
                node.options.forEach(option => {
                    const button = document.createElement('button');
                    button.className = 'flow-button';
                    // Use the pre-assigned unique ID (uid) for the label
                    button.textContent = `(${option.uid}) ${option.text}`;
                    button.onclick = () => {
                        renderNode(option.next);
                    };
                    buttonGrid.appendChild(button);
                });
                flowContentDiv.appendChild(buttonGrid);
                resetButton.classList.remove('hidden');
            } else if (node.decision) {
                // Display decision
                const decisionElement = document.createElement('p');
                decisionElement.className = 'decision-text mb-6';
                // Use innerText for pre-formatted text to preserve spaces and line breaks from template literal
                decisionElement.innerText = node.decision.text; 
                // Then replace ** for bolding using innerHTML
                decisionElement.innerHTML = decisionElement.innerHTML.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>');
                flowContentDiv.appendChild(decisionElement);
                
                // Show reset button on decision nodes
                resetButton.classList.remove('hidden');
            }
        }
        // Function to reset the flow entirely
        function resetFlow() {
            localStorage.removeItem('disclaimerAccepted');
            location.reload();
        }
        // --- Page Initialization Logic ---
        // Hide reset button initially
        resetButton.classList.add('hidden');
        // Add event listener for the reset button
        resetButton.addEventListener('click', resetFlow);
        // Check for disclaimer acceptance
        if (localStorage.getItem('disclaimerAccepted') === 'true') {
            disclaimerOverlay.classList.add('hidden');
            renderNode('user_type_check'); // Start the flow if already accepted
        } else {
            disclaimerOverlay.classList.remove('hidden');
        }
        // Add event listener for the accept disclaimer button
        acceptButton.addEventListener('click', () => {
            disclaimerOverlay.classList.add('hidden');
            localStorage.setItem('disclaimerAccepted', 'true');
            renderNode('user_type_check'); // Start the flow for the first time
        });
    </script>
</body>
</html>
