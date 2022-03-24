# Three.js 사용법

## **1. renderer**

화면을 그려주는애가 Renderer

**동적으로 캔버스 조립**

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize(window.innerWidth, window.innerHeight); // 사이즈를 창너비높이로 설정해줌.
    document.body.appendChild(renderer.domElement); // body의 자식으로 붙여줌.

**html내에 canvas태그를 사용한 경우**

    const canvas = document.querySelector('canvas태그의 셀렉터값');
    const renderer = new THREE.WebGLRenderer({
        canvas,
        antialias:true // 모서리가 계단처럼 각져있는것을 부드럽게 처리함
    });
    renderer.setSize(window.innerWidth, window.innerHeight); // 사이즈 설정

## **2. scene**

모든 컨텐츠는 scene위에 그려짐

    const scene = new THREE.Scene();

## **3. camera**

카메라는 시야각을 가짐<br>
Light(빛, 조명)는 필수는 아니지만 거의 대부분 사용함<br>
재질이 어떤것이느냐에 따라 조명은 필요할수도, 필요하지 않을수도 있음<br>

**PerspectiveCamera**

사람의 눈으로 보는 방식으로 3D 장면을 렌더링하는데 가장 널리 쓰임<br>
fov — 카메라 절두체 수직 시야 = 시야각(field of view)<br>
aspect — 카메라 절두체 종횡비<br>
near — 카메라 절두체 근평면<br>
far — 카메라 절두체 원평면<br>
near, far 를 숫자로 설정. 두값의 사이영역이 보여지는 영역<br>
카메라의 위치를 설정안하면 기본값은 (0, 0, 0)임<br>

    const camera = new THREE.PerspectiveCamera(
        75, // 시야각(field of view)
        window.innerWidth / window.innerHeight, // 종횡비(aspect)
        0.1, // near
        1000 // far
    );
    camera.position.x = 1; // 카메라와의 거리
    camera.position.y = 1; // 카메라와의 거리
    camera.position.z = 5; // 카메라와의 거리

**OrthographicCamera**

렌더링된 이미지에서 객체의 크기는 카메라와의 거리에 관계없이 일정하게 유지됨<br>
이 기능은 무엇보다도 2D 장면과 UI 요소를 렌더링하는 데 유용함<br>

    const camera = new THREE.OrthographicCamera(
        -(window.innerWidth / window.innerHeight), // left
        window.innerWidth / window.innerHeight, // right
        1, // top
        -1, // bottom
        0.1, // near
        1000 // far
    )
    camera.position.x = 1; // 카메라와의 거리
    camera.position.y = 2; // 카메라와의 거리
    camera.position.z = 5; // 카메라와의 거리
    camera.lookAt(0, 0, 0);
    // 줌아웃과 같은 효과를 주기 위해서는
    camera.zoom = 0.5; // zoom의 값을 변경해주고
    camera.updateProjectionMatrix(); // 업데이트를 해주어야함.

## **4. mesh**

오브젝트 하나하나를 mesh<br>
mesh는 Geomerty(모양) + Material(재질)로 이루어짐<br>

**geomerty**

    const geometry = new THREE.BoxGeometry(1, 1, 1);

**material**

    const material = new THREE.MeshBasicMaterial({
        color: 0x00ffff,
    });


설정한 geomerty(모양)과 material(재질)을 mesh에 반영<br>
설정한 mesh를 scene에 추가<br>
설정을 끝낸 scene와 camera를 renderer로 화면에 렌더링

    const mesh = new THREE.Mesh(geometry, material);
    scene.add(mesh);

    renderer.render(scene, camera);